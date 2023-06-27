# DynamicLink를 통한 공유하기기능 구현

[Document](https://firebase.google.com/docs/dynamic-links?hl=ko)

- dynamic link로 액세스한 사용자를 해당 콘텐츠로 바로 이동시키기 위해 사용
- 앱을 설치하지 않았다면 마켓으로 이동, 설치후 링크를 열면 앱이 시작되고 해당 콘텐츠에 액세스 한다


## 순서
0. 패키지 설치
1. Firebase 및 동적 링크 SDK 설정
2. 동적 링크 만들기
3. 동적 링크 수신하기
4. 링크를 통해 액세스하면 원하는 화면으로 이동시키기

---

### 0. 패키지 설치하기
- [Package](https://pub.dev/packages/firebase_dynamic_links)
- `flutter pub add firebase_dynamic_links` - `flutter pub get`

### 1. Firebase 및 동적 링크 SDK 설정
- Firebase Console -> 톱니바퀴 -> 프로젝트 설정에서 
Android `패키지 이름`, IOS `번들 ID`, `App Store ID` 확인
- Firebase Console -> Dynamic Links 
    1. url 프리픽스 추가 
        - 도메인(~~~.page.link) 입력 후 계속버튼 클릭 
        - 새 동적 링크 옆 점 3개 버튼 클릭
        - URL 패턴 허용 목록 만들기 (`^https://test\.page\.link.*$`)
        | [참고](https://stackoverflow.com/a/75088671)
    2. 새 동적 링크 버튼 클릭
        - `단축 URL` 링크 설정
        | 기본값 사용, Next
        - 허용한 패턴에 맞는 딥링크 설정
        | 앱에서 열릴 페이지를 url링크 및 이름 입력, Next
        - Apple용 링크 동작 정의 
        | Apple 앱에서 딥 링크 열기 체크 및 Apple 앱 추가, 앱의 App Store 페이지 체크, Next
        - Android용 링크 동작 정의 
        | Android 앱에서 딥 링크 열기 체크 및 Android 앱 추가, 
        | 앱의 Google Play 페이지 체크, Next
        - 캠페인 추적, 소셜 태그, 고급 옵션
        | 동적 링크에 소셜 메타데이터를 설정하면 미리보기 페이지에 적용이 됨 
        | [참고](https://firebase.google.com/docs/dynamic-links/link-previews?hl=ko)

### 2. 동적 링크 만들기
```dart
import 'package:firebase_dynamic_links/firebase_dynamic_links.dart';
import 'package:get/get_state_manager/src/simple/get_controllers.dart';


class ShareController extends GetxController{
  
  Future<Uri> postShareWithParams(String? type, dynamic params)async{

    dynamic type = type;
    dynamic param1 = params.param1;
    dynamic param2 = params.param2;

    final DynamicLinkParameters dynamicLinkParams = DynamicLinkParameters(
      uriPrefix: "https://test.page.link",
      link: Uri.parse("https://test.page.link/share?type=$type?param1=$param1?param2=$param2"),
      androidParameters: const AndroidParameters(
        packageName: // "패키지 이름",
      ),
      iosParameters: const IOSParameters(
        bundleId: // "번들 ID",
        appStoreId: // "앱스토어 ID",
      ),
      socialMetaTagParameters: SocialMetaTagParameters(
        title: // params.title,
        description: // params.dexcription,
        imageUrl: // Uri.parse(params.postMedia[0].thumbnailUrl)
      )
    );

    Uri dynamicLink = await FirebaseDynamicLinks.instance.buildLink(dynamicLinkParams);
    // ? shortLink는 공유시 풀url을 감추기위해 사용
    ShortDynamicLink shortLink = await FirebaseDynamicLinks.instance.buildShortLink(dynamicLinkParams);
    Uri replaceLink = Uri.parse(dynamicLink.toString().replaceAll('%3D','='));

    Share.share(shortLink.shortUrl.toString());

    return replaceLink;
  }
   
  // ...code
}
```
- replaceLink를 print하면 매개변수가 추가된 다이나믹 링크를 보여주고
이 링크를 누르면 SDK에서 설정해놓은 주소로 리다이렉션한다

### 3. 동적 링크 수신하기
- Android 설정
    -  `android/app/src/main/AndroidManifest.xml`에 추가
        ```xml
        <intent-filter>
            <action android:name="android.intent.action.VIEW"/>
            <category android:name="android.intent.category.DEFAULT"/>
            <category android:name="android.intent.category.BROWSABLE"/>
            <!-- host = 동적 링크 url -->
            <data
                android:host="test.page.link" 
                android:scheme="https"/>
        </intent-filter>
        ```
- IOS
    - Xcode -> Runner -> Signing & Capabilities
        - Associated Domains 추가
        - Domains에 `appliks:동적 링크 url`를 작성 (appliks:test.page.link)
    - Xcode -> Runner -> Info
        - URL Type 정의
        - identifier 필드에는 번들 ID, URL Schemes 필드에는 '동적 링크 url' 입력(test.page.link)
    - `!` ios는 `flutter run --release`를 통해 릴리즈 모드로 실행해야 테스트할 수 있음
<br>
- [app_links](https://pub.dev/packages/app_links) 패키지 설치
  > 설치 이유? : 
  > Android는 종료된 상태에서 항상 `FirebaseDynamicLinks.getInitialLink`를 통해 링크를 수신하지만 
  > iOS에서는 보장되지 않기 때문에 app_links 패키지를 사용해서 문제를 해결해준다

<br>
- 링크 수신 코드 작성

```dart
class ShareController extends GetxController{
  
  // ...code 

  Future<void> setup() async {

    // 앱이 종료상태일 경우 (Terminate)
    final PendingDynamicLinkData? initialLink = await FirebaseDynamicLinks.instance.getInitialLink();
    final _appLinks = AppLinks();
    if (initialLink != null) {
      final Uri deepLink = initialLink.link;
      onListenLink(deepLink);
    } else {
      // * [app_links] 사용
      final Uri? uri = await _appLinks.getInitialAppLink();
      if (uri != null) {
        _appLinks.uriLinkStream.listen(
          (uri) {
            onListenLink(uri);
          }
        );
      }
    }

    // 백그라운드 / 포어그라운드 상태
    FirebaseDynamicLinks.instance.onLink.listen(
      (PendingDynamicLinkData dynamicLinkData,) {
        onListenLink(dynamicLinkData.link);
      }
    ).onError((error) {
      print('Share : Background / Foreground ERROR ===== $error');
    });

  }

  //-----------------------------------------------------

  // 화면이동 구현
  void onListenLink(dynamic params) async{
    
    params = params.toString().replaceAll('%3D','=');

    // query에서 type구하기
    String type = await params.substring(
      params.indexOf('type=')+5, // type= 의 length가 5라서 더해줌
      params.indexOf('?', params.indexOf('type'))
    );
    // query에서 param1 구하기
    String senderId = await params.substring(
      params.indexOf('param1=')+7, // param1= 의 length가 7이라서 더해줌
      params.indexOf('?', params.indexOf('param1'))
    );
    // query에서 param2 구하기
    int number = await int.parse(
      params.substring(
        params.indexOf('param2=')+7, // param2= 의 length가 7이라서 더해줌
        params.length
      )
    );
    
    switch (type) {
      case 'type1':
        // type이 type1일 경우 화면이동
        print('type = $type, param1 = $param1, param2 = $param2')
        break;

      case 'type2':
        // type이 type2일 경우 화면이동
        print('type = $type, param1 = $param1, param2 = $param2')
        break;
    }
  }
}
```
=> `setup()`을 화면을 그려주는 위젯의 initState에 넣어주면 됨 (ex. main.dart)
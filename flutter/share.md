# DynamicLink를 통한 공유하기기능 구현

- dynamic link를 통해 액세스한 사용자를 앱 내의 해당 콘텐츠로 바로 이동시키기 위해 사용
- 앱을 설치하지 않았다면 마켓으로 이동, 설치후 링크를 열면 앱이 시작되고 해당 콘텐츠에 액세스 한다

[Document](https://firebase.google.com/docs/dynamic-links?hl=ko)

## 순서
0. 패키지 설치
1. Firebase 및 동적 링크 SDK 설정
2. 동적 링크 만들기
3. 동적 링크 수신하기
3-1. 링크를 통해 액세스하면 원하는 화면으로 이동시키기

---

### 0. 패키지 설치하기
- [Package](https://pub.dev/packages/firebase_dynamic_links)
- `flutter pub add firebase_dynamic_links` - `flutter pub get`

### 1. Firebase 및 동적 링크 SDK 설정
- Firebase Console -> 톱니바퀴 -> 프로젝트 설정에서 Android의 `패키지 이름`, IOS의 `번들 ID`, `App Store ID`를 확인
- Firebase Console -> Dynamic Links 
    1. url 프리픽스 추가 
        - 도메인(~~~.page.link) 입력 후 계속버튼 클릭 
        - 새 동적 링크 옆 점 3개 버튼 클릭
        - URL 패턴 허용 목록 만들기 (`^https://test\.page\.link.*$`)
    | [참고](https://stackoverflow.com/a/75088671)
    2. 새 동적 링크 버튼 클릭
        - `단축 URL` 링크 설정
        : 기본값 사용, Next
        - 허용한 패턴에 맞는 딥링크 설정
        : 앱에서 열릴 페이지를 url링크 및 이름 입력, Next
        - Apple용 링크 동작 정의 
        : Apple 앱에서 딥 링크 열기 체크 및 Apple 앱 추가, 앱의 App Store 페이지 체크, Next
        - Android용 링크 동작 정의 
        : Android 앱에서 딥 링크 열기 체크 및 Android 앱 추가, 앱의 Google Play 페이지 체크, Next
        - 캠페인 추적, 소셜 태그, 고급 옵션
        : 동적 링크에 소셜 메타데이터를 설정하면 미리보기 페이지에 적용이 됨 
        | [참고](https://firebase.google.com/docs/dynamic-links/link-previews?hl=ko)

### 2. 동적 링크 만들기
```dart
import 'package:firebase_dynamic_links/firebase_dynamic_links.dart';
import 'package:get/get_state_manager/src/simple/get_controllers.dart';


class Share extends GetxController{
  Future<Uri> contentsWithParams(String? type, dynamic numb)async{

    dynamic contentsType = type;
    dynamic number = numb;

    final DynamicLinkParameters dynamicLinkParams = DynamicLinkParameters(
      uriPrefix: "https://test.page.link",
      link: Uri.parse("https://test.page.link/share?contentsType=$contentsType?number=$number"),
      androidParameters: const AndroidParameters(
        packageName: // Android 패키지 이름,
      ),
      iosParameters: const IOSParameters(
        bundleId: // ios 빌드 ID,
        appStoreId: // 앱스토어 ID,
      ),
    );

    Uri dynamicLink = await FirebaseDynamicLinks.instance.buildLink(dynamicLinkParams);
    return dynamicLink;
  }
}
```
- dynamicLink를 print하면 다이나믹 링크 매개변수가 추가된 링크를 보여주고 이 링크로 가면 다이나믹 링크 SDK에서 설정해놓은 주소로 리다이렉션해줌

### 3. 동적 링크 수신하기
- Android / IOS 설정
    - Android
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
- 링크 수신 코드 작성



---
- 참고사항(다이나믹 링크 매개변수)
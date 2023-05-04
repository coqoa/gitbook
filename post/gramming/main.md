# 진입점 역할을 하는 main 페이지 코드 설명
- main.dart

    <details>

    <summary>전체코드</summary>

    ```dart
        // 진입점 역할을 하는 파일

        import 'package:bwa/apikey.dart';
        import 'package:bwa/screen/menu.dart';
        import 'package:bwa/screen/sign.dart';
        import 'package:bwa/widget/language.dart';
        import 'package:firebase_auth/firebase_auth.dart';
        import 'package:flutter/material.dart';
        import 'package:flutter_screenutil/flutter_screenutil.dart';
        import 'package:get/get.dart';
        import 'package:flutter/services.dart';
        import 'package:firebase_core/firebase_core.dart';

        void main() async {

            FIREBASE_API_KEYS firebaseOptions = FIREBASE_API_KEYS(); 

            try{
                WidgetsFlutterBinding.ensureInitialized();
                await Firebase.initializeApp(
                    options: FirebaseOptions(
                        apiKey: firebaseOptions.apiKey,
                        authDomain: firebaseOptions.authDomain,
                        projectId: firebaseOptions.projectId,
                        storageBucket: firebaseOptions.storageBucket,
                        messagingSenderId: firebaseOptions.messagingSenderId,
                        appId: firebaseOptions.appId,
                        measurementId: firebaseOptions.measurementId
                    ),
                );
            }catch(e){
                print('-- main.dart initializeApp ERROR -- $e');
            }

            SystemChrome.setPreferredOrientations([
                DeviceOrientation.portraitUp
            ]).then((value) => 
                runApp(const MyApp())
            );
        }

        class MyApp extends StatefulWidget {
            const MyApp({Key? key}) : super(key: key);

            @override
            State<MyApp> createState() => _MyAppState();
        }

        class _MyAppState extends State<MyApp> {

            @override
            void initState() {
                super.initState();
            }

            @override
            Widget build(BuildContext context) {

                SystemChrome.setEnabledSystemUIMode(
                    SystemUiMode.manual,
                    overlays: [
                        SystemUiOverlay.top,
                    ],
                );

                return ScreenUtilInit(
                    designSize: const Size(360, 880),
                    minTextAdapt: true,
                    splitScreenMode: true,
                    builder: (context , child) { 
                        return GetMaterialApp(
                            title: 'Flutter Demo',
                            debugShowCheckedModeBanner: false,
                            translations: Languages(),
                            locale: Get.deviceLocale,
                            fallbackLocale: const Locale('en', 'US'),
                            theme: ThemeData(
                                fontFamily: 'nanumBarun',
                                brightness: Brightness.light,
                                backgroundColor: Colors.white,
                                visualDensity: VisualDensity.adaptivePlatformDensity,
                                scaffoldBackgroundColor: Colors.white,
                            ),
                        
                            builder: (context, child){
                                return MediaQuery(
                                data: MediaQuery.of(context).copyWith(textScaleFactor: 1.0), 
                                child: child!
                                );
                            },
                        
                            home: FirebaseAuth.instance.currentUser?.uid != null 
                                ? Menu() 
                                : Sign(),
                        );
                    },
                
                );
            }
        }
        ```
    </details>
  
<br>

- 각개 설명 
  
<br>

```dart

// 외부로부터 감춰야 하는 고유한 key들을 관리하는 파일 'apikey.dart'를 사용

FIREBASE_API_KEYS firebaseOptions = FIREBASE_API_KEYS(); 

try{
    WidgetsFlutterBinding.ensureInitialized();
    await Firebase.initializeApp(
        options: FirebaseOptions(
            apiKey: firebaseOptions.apiKey,
            authDomain: firebaseOptions.authDomain,
            projectId: firebaseOptions.projectId,
            storageBucket: firebaseOptions.storageBucket,
            messagingSenderId: firebaseOptions.messagingSenderId,
            appId: firebaseOptions.appId,
            measurementId: firebaseOptions.measurementId
        ),
    );
}catch(e){
    print(e);
}
```

<br>

```dart
// * 참고 : apikey.dart 예시

import 'package:flutter/foundation.dart';

class FIREBASE_API_KEYS{
    final apiKey = "apiKey";
    final  authDomain = "authDomain";
    final  projectId = "projectId";
    final  storageBucket = "storageBucket";
    final  messagingSenderId = "messagingSenderId";
    final  appId = "appId";
    final  measurementId = "measurementId";
}
```

<br>

```dart
// 앱이 실행되기 전에 'Flutter Engine'이 초기화되도록 보장하는 메소드
WidgetsFlutterBinding.ensureInitialized(); 
```

<br>

```dart
// 앱의 화면 방향을 제한하는데 사용하는 메소드
SystemChrome.setPreferredOrientations([
    DeviceOrientation.portraitUp // 화면방향을 세로로 고정시키는 속성
]).then((value) => 
    runApp(const MyApp()) // runApp()은 앱을 실행하는 역할을 하는 메소드로
);
```

<br>

```dart
// 앱이 실행되기 전에 'Flutter Engine'이 초기화되도록 보장하는 메소드
WidgetsFlutterBinding.ensureInitialized(); 
```

<br>

```dart
// 상단바만 보이게 하고 하단 네비게이션 바는 숨겨줌
SystemChrome.setEnabledSystemUIMode(
    SystemUiMode.manual,
    overlays: [
        SystemUiOverlay.top,
    ],
);
```

<br>

```dart
// 위젯 크기를 일정하게 관리하기 위한 패키지 ScreenUtilInit
ScreenUtilInit(
    designSize: const Size(360, 880), // 기준 크기를 지정,
    minTextAdapt: true, // 너비와 높이의 최소값에 따라 텍스트를 조정할지 여부
    splitScreenMode: true, // 분할 화면 지원
    builder: (context , child) { 
        return GetMaterialApp(
            title: 'Flutter Demo',
            debugShowCheckedModeBanner: false, // debug 표시 여부
            translations: Languages(), // 번역될 언어들을 담아둔 클래스
            locale: Get.deviceLocale, // 현재 기기위치를 기본으로 사용
            fallbackLocale: const Locale('en', 'US'), // 지원하지 않는 언어를 사용할 시 나타날 기본 언어
            theme: ThemeData(
                fontFamily: 'nanumBarun',
                brightness: Brightness.light,
                backgroundColor: Colors.white,
                visualDensity: VisualDensity.adaptivePlatformDensity,
                scaffoldBackgroundColor: Colors.white,
            ),
        
            builder: (context, child){
                return MediaQuery(
                data: MediaQuery.of(context).copyWith(textScaleFactor: 1.0),  //폰트 크기 고정하기
                child: child!
                );
            },
        
            home: FirebaseAuth.instance.currentUser?.uid != null 
                ? Menu() // FirebaseAuth가 있으면 Menu
                : Sign(), // FirebaseAuth가 없으면 Sign()을 출력
        );
    },

);
```
---

<br>

## 추후에 추가 설명할 토픽 : 
- Firebase
- ScreenUtilInit
- GetX

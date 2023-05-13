# Flutter Fidrebase
[doc](https://firebase.google.com/docs/flutter/setup?hl=ko&platform=ios)
## # Firebase 프로젝트 생성

- [link](https://firebase.google.com)

- 로그인  
`  >  `콘솔로 이동  
`  >  `프로젝트 만들기  
`  >  `프로젝트 이름 입력  
`  >  `Google 애널리틱스 사용 여부 선택 및 설정  
`  >  `프로젝트 생성

<br>
 
## # iOS 설정
1. Firebase개요 > '+앱추가' > ios
2. Xcode > General > Bundle Identifier 변경하고 복사해놓기
3. 프로젝트 선택 > iOS버튼 클릭
4. 복사해놓은 Bundle Identifier 입력 > 앱 등록 
5. `GoogleService-info.plist` 파일 다운로드 > 'ios/Runner' 내부에 넣기
6. firestore sdk 추가 > next 
7. 나머지 설정 적용

<br>
 
## # Android 설정
1. Firebase개요 > '+앱추가' > Android
2. 패키지 이름 입력 ('android/app/build.gradle' 내부의 applicationId) > 앱 등록
3. `google-services.json`파일 다운로드 > 'android/app' 내부에 넣기
4. firestore sdk 추가 > next 
    ```dart
    // android/build.gradle

    buildscript {
        repositories {
            
            google()  
            mavenCentral()  

        }
        dependencies {
            ...
            classpath 'com.google.gms:google-services:4.3.15'
        }
    }

    allprojects {
        ...
        repositories {
            google() 
            mavenCentral()
        }
    }
    ```
    ```dart
    // android/app/build.gradle

    apply plugin: 'com.google.gms.google-services' 

    applicationId "com.coqoa.gramming"

    dependencies {
        implementation platform('com.google.firebase:firebase-bom:31.5.0')
        implementation 'com.android.support:multidex:2.0.1'
    }
    ```
5. 나머지 설정 적용

<br>

## # firebase_core 설치  
- [link](https://pub.dev/packages/firebase_core)

- 설치  
  `flutter pub add firebase_core`

- Podfile 수정
    ```dart
    // ios/Podfile
    platform :ios, '11.0'
    ```
    `cd ios`  
    `pod install`
- Firebase 초기화
    ```dart
    // main.dart
    import 'package:firebase_core/firebase_core.dart';

    Future<void> main() async {
        WidgetsFlutterBinding.ensureInitialized();
        await Firebase.initializeApp();

        runApp(MyApp());
    }
    ```


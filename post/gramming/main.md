# 진입점 역할을 하는 main

<br>

## # 전체 코드

<details>

<summary>main.dart</summary>

```dart


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

## # 코드 설명 
  
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

<details>
<summary> 참고 : apikey.dart 예시</summary>

```dart
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

</details>

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

<br>

<details>
<summary> 참고 : language.dart 예시</summary>

```dart
import 'package:get/get.dart';

class Languages extends Translations {
  @override
  Map<String, Map<String, String>> get keys => {
        'ko_KR': {

          // sign.dart
          'eMailAddress' : '이메일',
          'password' : '비밀번호',
          'passwordRepeat' : '비밀번호 확인',
          'logIn' : '로그인',
          'register' : '회원가입',
          'dontYouHaveAnAccount' : '계정이 없으세요? ',
          'joinUs' : '회원가입하기',
          'doYouHaveAnAccount' : '계정이 있으세요? ',
          'logIn2' : '로그인하기',
          'orStartWithAnAnonymousAccount' : '일단 회원가입 없이 시작할래요',

          // sign_controller.dart
          'passwordInvalid' : '잘못된 비밀번호 입니다',
          'badlyFormatEmail' : '이메일을 형식에 맞게 입력해주세요',
          'thereIsNoUserRecord' : '회원 정보가 없습니다',
          'anEmailAddressMustBeProvided' : '이메일 주소는 필수 입력사항 입니다',
          'alreadyInUseEmail' : '사용중인 이메일 입니다',
          'atLeast6CharactersPassword' : '비밀번호는 6자 이상으로 작성해주세요',
          'passwordsDoNotMatch' : '패스워드가 맞지 않습니다',

          // menu.dart
          'ifYouStartWithoutLoggingIn' : '회원가입 없이 시작하면',
          'youMayLoseYourData' : '데이터가 삭제될 수도 있습니다',
          'toJoinGrammingTapHere' : 'Gramming에 가입하려면 여기를 눌러주세요!',
          'createMenu' : '메뉴 만들기',
          'create' : '생성',
          'back' : '뒤로',
          'submit' : '확인',
          'createNewMenu' : '새로운 Menu 만들기',
          'edit' : '수정',
          'editRecipe' : '레시피 수정',
          'delete' : '삭제',
          'areYouSureDelete1' : '정말로 ',
          // 타이틀 들어갈 곳
          'areYouSureDelete2' : '를 삭제할까요',
          // 2와 3은 붙여쓰면 됨
          'areYouSureDelete3' : '?',
          'joinUs2' : '회원가입',
          'secession1': '회원탈퇴',
          'secession2': '',
          'secession3': '정말로 탈퇴 하시겠습니까?',
          'translate': '한/영 전환',
          'logout' : '로그아웃',

          // menu_controller.dart
          'error' : '에러',
          'anAlreadyExistingTitle' : '이미 존재하는 제목입니다',
          'pleaseCheckTheTitle' : '제목을 확인해주세요',
          'isAlreadyExistsTitle' : ' 는 이미 존재하는 제목입니다',
          // validation
          'pleaseCheckYourPassword' : '비밀번호를 확인해주세요',
          'passwordDoesNotMatch' : '비밀번호가 일치하지 않습니다',
          'theProviderHasAlreadyBeenLinkedToTheUser' : '이미 연결된 이메일입니다',
          "theProvidersCredentialIsNotValid" : "유효하지 않습니다",
          "theAccountCorrespondingToTheCredentialAlreadyExistsOrIsAlreadyLinkedToAFirebaseUser" : "이미 존재하는 이메일입니다",
          "thisEmailIsInUse" : "이미 사용중인 이메일입니다",
          "pleaseEnterInEmailFormat" : "이메일 형식으로 입력해주세요",
          "pleaseCheckYourPasswordPasswordMustBeDigitsOrMore" : "비밀번호를 확인해주세요 \n 비밀번호는 6자리 이상의 숫자여야 합니다 ",
          "errorCode305" : "Error Code:305",
          
          // recipe.dart
          'memo' : '메모',
          'add' : '추가',
          'addAnewRecipe' : '레시피 추가',
          'ingredient' : '재료',
          'total' : '총 합 ',
          'multiply' : '곱하기',
          'divide' : '나누기',
          // 'edit' : 'Edit', // 위에있음
          'areYouSureDelete' : '정말 삭제하시겠습니까?',
          // 'delete' : 'Delete',// 위에있음

          // recipe_controller.dart
          'isAlreadyExists' : ' 은/는 이미 존재합니다.',
          'pleaseEnterATitle' : '제목을 입력해주세요',

          // add_recipe.dart
          'title' : '제목',

          // calculator.dart
          'calculator' : '계산기',

          // memo.dart
          'save' : '저장', //! 통합?
          'enterWithinCharacters' : '500자 이내로 입력해주세요',
          

        },
        'en_US': {

          // sign.dart
          'eMailAddress' : 'E-Mail Address',
          'password' : 'Password',
          'passwordRepeat' : 'Password Repeat',
          'logIn' : 'Log in',
          'register' : 'Register',
          'dontYouHaveAnAccount' : 'Don’t you have an account? ',
          'joinUs' : 'Join us!',
          'doYouHaveAnAccount' : 'Do you have an account? ',
          'logIn2' : 'Log in!',
          'orStartWithAnAnonymousAccount' : 'or Start with an anonymous account',

          // signController.dart
          'passwordInvalid' : 'password invalid',
          'badlyFormatEmail' : 'badly format email',
          'thereIsNoUserRecord' : 'There is no user record',
          'anEmailAddressMustBeProvided' : 'An email address must be provided',
          'alreadyInUseEmail' : 'already in use email',
          'atLeast6CharactersPassword' : 'at least 6 characters password',
          'passwordsDoNotMatch' : 'Passwords do not match',

          // menu.dart
          'ifYouStartWithoutLoggingIn' : 'If you start without signup,',
          'youMayLoseYourData' : 'you may lose your data',
          'toJoinGrammingTapHere' : 'To join Gramming, tap here!',
          'createMenu' : 'Create menu',
          'create' : 'Create',
          'back' : 'Back',
          'submit' : 'Submit',
          'createNewMenu' : 'Create new menu',
          'edit' : 'Edit',
          'editRecipe' : 'Edit recipe',
          'delete' : 'Delete',
          'areYouSureDelete1' : 'Are you sure delete',
          'areYouSureDelete2' : '',
          'areYouSureDelete3' : '?',
          'joinUs2' : 'Join Us',
          'secession1': 'Secession',
          'secession2': '',
          'secession3': 'Are you sure you want to secession?',
          'logout' : 'Logout',
          'translate': 'Switch to Kor',

          // menu_controller.dart
          'error' : 'ERROR',
          'anAlreadyExistingTitle' : 'An already existing Title',
          'pleaseCheckTheTitle' : 'Please check the Title',
          'isAlreadyExistsTitle' : ' is already exists Title',
          // validation
          'pleaseCheckYourPassword' : 'Please check your password',
          'passwordDoesNotMatch' : 'Password does not match',
          'theProviderHasAlreadyBeenLinkedToTheUser' : 'The provider has already been linked to the user',
          "theProvidersCredentialIsNotValid" : "The provider's credential is not valid",
          "theAccountCorrespondingToTheCredentialAlreadyExistsOrIsAlreadyLinkedToAFirebaseUser" : "The account corresponding to the credential already exists, \nor is already linked to a Firebase User",
          "thisEmailIsInUse" : "This email is in use",
          "pleaseEnterInEmailFormat" : "Please enter in email format",
          "pleaseCheckYourPasswordPasswordMustBeDigitsOrMore" : "Please check your password \nPassword must be 6 digits or more",
          "errorCode305" : "Error Code:305",
          
          // ! sign, menu 스크린, 컨트롤러 확인해고 recipe 하러 넘어가기

          // recipe.dart
          'memo' : 'Memo',
          'add' : 'Add',
          'addAnewRecipe' : 'Add a new Recipe',
          'ingredient' : 'Ingredient',
          'total' : 'Total ',
          'multiply' : 'Multiply',
          'divide' : 'Divide',
          // 'edit' : 'Edit', // 위에있음
          'areYouSureDelete' : 'Are you sure delete ?',
          // 'delete' : 'Delete',// 위에있음

          // recipe_controller.dart
          'isAlreadyExists' : ' is already exists',
          'pleaseEnterATitle' : 'Please enter a Title',

          // add_recipe.dart
          'title' : 'Title',
          // edit_recipe.dart
          // calculator.dart
          'calculator' : 'Calculator',
          // memo.dart
          'save' : 'Save', //! 통합?
          'enterWithinCharacters' : 'Enter within 500 characters',

        },
      };
}
```  

</details>

<br>

---

<br>

## 추후에 추가 설명할 토픽 : 
- Firebase
- ScreenUtilInit
- GetX

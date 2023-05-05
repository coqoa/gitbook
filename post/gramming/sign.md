# 회원 가입 페이지

<br>

## # 전체 코드


 
<details>

<summary>sign_controller.dart</summary>

```dart
import 'package:bwa/screen/menu.dart';
import 'package:firebase_auth/firebase_auth.dart';
import 'package:get/get.dart';

class SignController extends GetxController{

  final _authentication = FirebaseAuth.instance;

  RxString userEmail = ''.obs;
  RxString userPassword = ''.obs;
  RxString userPasswordRepeat = ''.obs;
  RxString validationResult = ''.obs;

  Future<void> initValidation()async {
    validationResult.value = '';
  }

  Future<void> textFieldChanged(type, value)async {
    validationResult.value = '';

    if(type == 'userEmail'){ 
      userEmail.value = value;
    }else if (type == 'userPassword'){
      userPassword.value = value;
    }else if (type == 'userPasswordRepeat'){
      userPasswordRepeat.value = value;
    }
  }

  // 검증 결과 출력 메소드
  Future<void> validation(String errMsg, String sign)async {
    if(sign == 'SignIn'){
      // * 로그인 검증 결과
      // 패스워드 오류
      if(errMsg == 'The password is invalid or the user does not have a password.'){
        validationResult.value = 'passwordInvalid'.tr;
      }
      // 이메일 형식 오류
      else if(errMsg == 'The email address is badly formatted.'){
        validationResult.value = 'badlyFormatEmail'.tr;
      }
      // 없는 이메일
      else if(errMsg == 'There is no user record corresponding to this identifier. The user may have been deleted.'){
        validationResult.value = 'thereIsNoUserRecord'.tr;
      }

    }else{
      // * 회원가입 검증 결과
      if(userPassword == userPasswordRepeat){
        // 이메일 형식 오류
        if(errMsg == '[firebase_auth/invalid-email] The email address is badly formatted.'){
          validationResult.value = 'badlyFormatEmail'.tr;
        }
        // 이메일 입력 요망
        else if(errMsg == '[firebase_auth/missing-email] An email address must be provided.'){
          validationResult.value = 'anEmailAddressMustBeProvided'.tr;
        } 
        // 사용중인 이메일
        else if(errMsg == '[firebase_auth/email-already-in-use] The email address is already in use by another account.'){
          validationResult.value = 'alreadyInUseEmail'.tr;
        }
        // 비밀번호 길이 오류
        else if(errMsg == '[firebase_auth/weak-password] Password should be at least 6 characters'){
          validationResult.value = 'atLeast6CharactersPassword'.tr;
        }
      }else{
        // 틀린 비밀번호
        validationResult.value = 'passwordsDoNotMatch'.tr;
      }
    }
  }

  // 회원가입
  Future<void> signIn(String sign)async {
    await _authentication.signInWithEmailAndPassword(
      email: userEmail.value, 
      password: userPassword.value
    ).then((value) {
      if(value.user != null){Get.to(()=> Menu());}
    }).catchError((e)async{
      await validation(e.message, sign);
    });
  }

  // 회원가입검증
  Future<void> signUp(String sign)async {
    try{
      if(userPassword.value == userPasswordRepeat.value){
        final newUser = await _authentication.createUserWithEmailAndPassword(
          email: userEmail.value, 
          password: userPassword.value,
        );
        if(newUser.user != null){Get.to(()=> Menu());}
      }else{
        validation('Passwords do not match', sign);
      }

    }catch(e){
      print('SIGNUP ERROR = $e');
      // 검증 결과 넣어주는 메소드
      await validation(e.toString(), sign);
    }
  }

  // * 익명로그인
  startAnonymous()async{
    _authentication.signInAnonymously().then((value) => Get.to(()=> Menu()));
  }
}
```

</details>

<br>
 
<details>

<summary>sign.dart</summary>

```dart
import 'package:bwa/config/palette.dart';
import 'package:bwa/controller/main_controller.dart';

import 'package:bwa/controller/sign_controller.dart';
import 'package:flutter/material.dart';
import 'package:flutter_keyboard_visibility/flutter_keyboard_visibility.dart';
import 'package:flutter_screenutil/flutter_screenutil.dart';
import 'package:get/get.dart';

class Sign extends StatefulWidget {
  Sign({Key? key}) : super(key: key);
  @override
  State<Sign> createState() => _SignState();
}

class _SignState extends State<Sign> {
  bool isSignin = true;
  bool isBtnHovered = false;
  late double statusBarHeight = MediaQuery.of(context).padding.top; // 상단 바 크기
  final SignController controller = SignController(); 

  @override
  void initState() {
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return WillPopScope(
      onWillPop: () async => false, 
      child: Scaffold(
        body: KeyboardVisibilityBuilder(
          builder: (context, isKeyboardVisible) { 
            return SafeArea(
              child: Stack(
                children: [
                  // header:
                  Align(
                    alignment: Alignment.topCenter,
                    child: AnimatedContainer(
                      duration: Duration(milliseconds: 200),
                      curve: Curves.easeIn,
                      width: 300.w,
                      height: isKeyboardVisible ? 0 : 370.h,
                      child: Center(
                        child: 
                        // section: 로고 이미지
                        Stack(
                          children: <Widget>[
                            Text(
                              'Gramming',
                              style: TextStyle(
                                fontFamily: 'carter',
                                fontWeight: FontWeight.w900,
                                fontSize: 40,
                                // 텍스트 테두리 선
                                foreground: Paint()
                                  ..style = PaintingStyle.stroke
                                  ..strokeWidth = 4
                                  ..color = Colors.black,
                                letterSpacing: 5,
                              ),
                            ),
                            Text(
                              'Gramming',
                              style: TextStyle(
                                fontFamily: 'carter',
                                fontWeight: FontWeight.w900,
                                fontSize: 40,
                                color: Palette.white,
                                letterSpacing: 5,
                                shadows: const <Shadow>[
                                  Shadow(
                                    offset: Offset(4.0, 3.0),
                                    blurRadius: 0,
                                    color: Color.fromARGB(255, 0, 0, 0),
                                  ),
                                ],
                              ),
                            ),
                          ],
                        )
                      )
                    ),
                  ),
                  
                  // main:
                  Align(
                    alignment: Alignment.bottomCenter,
                    child: Container(
                      width: 360.w,
                      height: 520.h,
                      color: Colors.transparent,
                      child: Column(
                        mainAxisAlignment: MainAxisAlignment.center,
                        children: [
                          // section: 회원가입 / 로그인
                          Container(
                            width: 244.w,
                            height: 410.h,
                            decoration: BoxDecoration(
                              color: Palette.white,
                              borderRadius: BorderRadius.circular(5),
                            ),
                      
                            child: Column(
                              mainAxisAlignment: MainAxisAlignment.spaceBetween,
                              children: [
                                // 메인 텍스트 필드
                                Column(
                                  children: [
                                    SizedBox(
                                      height: 220.h,
                                      child: isSignin 
                                      // Sign in Box
                                      ? Column(
                                        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                                        children: [
                                          SignTextField(
                                            controller: controller,
                                            valueKey: const ValueKey(1), 
                                            obscureText: false, 
                                            hintText: 'eMailAddress'.tr, 
                                            type: 'userEmail',
                                            textInputAction: TextInputAction.next,
                                            nextEvent: true,
                                            sign: isSignin,
                                          ),
                                          SignTextField(
                                            controller: controller,
                                            valueKey: const ValueKey(2), 
                                            obscureText: true, 
                                            hintText: 'password'.tr, 
                                            type: 'userPassword',
                                            textInputAction: TextInputAction.done,
                                            nextEvent: false,
                                            sign: isSignin,
                                          ),
                                        ],
                                      )
                      
                                      // Sign up Box
                                      : Column(
                                        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                                        children: [
                                          SignTextField(
                                            controller: controller,
                                            valueKey: const ValueKey(3), 
                                            obscureText: false, 
                                            hintText: 'eMailAddress'.tr, 
                                            type: 'userEmail',
                                            textInputAction: TextInputAction.next,
                                            nextEvent: true,
                                            sign: isSignin,
                                          ),
                                          SignTextField(
                                            controller: controller,
                                            valueKey: const ValueKey(4), 
                                            obscureText: true, 
                                            hintText: 'password'.tr, 
                                            type: 'userPassword',
                                            textInputAction: TextInputAction.next,
                                            nextEvent: true,
                                            sign: isSignin,
                                          ),
                                          SignTextField(
                                            controller: controller,
                                            valueKey: const ValueKey(5), 
                                            obscureText: true, 
                                            hintText: 'passwordRepeat'.tr, 
                                            type: 'userPasswordRepeat',
                                            textInputAction: TextInputAction.done,
                                            nextEvent: false,
                                            sign: isSignin,
                                          ),
                                        ],
                                      ),
                                    ),
                                  ],
                                ),
                                
                                // 하단 
                                SizedBox(
                                  width: 300.w,
                                  height: 180.h,
                                  child: Column(
                                    mainAxisAlignment: MainAxisAlignment.spaceAround,
                                    children: [
                                      // 에러 메시지
                                      Obx(()=>
                                        SizedBox(
                                          height: 25.h,
                                          child: Center(
                                            child: Text(controller.validationResult.value,
                                              style: TextStyle(
                                                fontSize: 14,
                                                fontWeight: FontWeight.w400,
                                                color: Palette.red
                                              ),
                                            ),
                                          ),
                                        )
                                      ),
    
                                      SizedBox(height: 10,),
    
                                      // Submit 버튼
                                      InkWell(
                                        onTap: () {
                                          isSignin ? controller.signIn('SignIn') : controller.signUp('SignUp');
                                        },
                                        child: Container(
                                          width: 214.w,
                                          height: 60.h,
                                          decoration: BoxDecoration(
                                            color: Palette.black,
                                            borderRadius: BorderRadius.circular(10),
                                          ),
                                          child: Center(
                                            child: Text(isSignin ? 'logIn'.tr : 'register'.tr,
                                            style: TextStyle(
                                              color: Colors.white,
                                              fontWeight: FontWeight.w700,
                                              fontSize: 18
                                            ),),
                                          ), 
                                        ),
                                      ),
    
                                      SizedBox(height: 10,),
                      
                                      // 회원가입 / 로그인 토글 버튼
                                      InkWell(
                                        onTap: (){
                                          setState(() {
                                            isSignin = !isSignin;
                                            controller.initValidation();
                                          });
                                        },
                                        child: SizedBox(
                                          height: 25.h,
                                          child: Row(
                                            mainAxisAlignment: MainAxisAlignment.center,
                                            children: [
                                              Text(
                                                isSignin ? 'dontYouHaveAnAccount'.tr : 'doYouHaveAnAccount'.tr,
                                                style: TextStyle(
                                                  fontSize: 14,
                                                  fontWeight: FontWeight.w800,
                                                  color: Palette.middleblack
                                                ),
                                              ),
                                              Text(isSignin ?'joinUs'.tr :'logIn2'.tr,
                                                style: TextStyle(
                                                  color: Palette.blue,
                                                  fontSize: 13,
                                                  fontWeight: FontWeight.w900,
                                                  fontStyle: FontStyle.italic
                                                ),
                                              )
                                            ],
                                          ),
                                        ),
                                      ),

                                      SizedBox(height: 10,),

                                      // 익명 로그인 버튼
                                      InkWell(
                                        onTap: (){
                                          controller.startAnonymous();
                                        },
                                        child: SizedBox(
                                          height: 25.h,
                                          child: Text(
                                            'orStartWithAnAnonymousAccount'.tr,
                                            style: TextStyle(
                                              fontSize: 14,
                                              fontWeight: FontWeight.w800,
                                              color: Palette.gray
                                            ),
                                          ),
                                        ),
                                      )
                                    ],
                                  )
                                ),
                              ],
                            ),
                          ),
                        ],
                      ),
                    ),
                  ),
                  Positioned(
                    bottom: 20,
                    right: 20,
                    child: MainController().mainController()
                  )
                ],
              ),
            );
          },
        ),
      ),
    );
  }
}

class SignTextField extends StatefulWidget {
  SignTextField({
    super.key, 
    required this.controller, 
    required this.valueKey, 
    required this.obscureText, 
    required this.hintText, 
    required this.type, 
    required this.textInputAction, 
    required this.nextEvent, 
    required this.sign
  });

  final SignController controller;
  final ValueKey valueKey;
  final bool obscureText;
  final String hintText;
  late String type;
  final TextInputAction textInputAction;
  final bool nextEvent;
  final bool sign;

  @override
  State<SignTextField> createState() => _SignTextFieldState();
}

class _SignTextFieldState extends State<SignTextField> {
  @override
  Widget build(BuildContext context) {
    return TextField(
      key: widget.valueKey, 
      keyboardType: TextInputType.emailAddress,
      obscureText: widget.obscureText,
    
      cursorColor: Palette.black,
      cursorWidth: 2,
      cursorHeight: 20,
      autocorrect: false, 

      textInputAction: widget.textInputAction,
    
      style: const TextStyle(
        fontSize: 20,
        fontWeight: FontWeight.bold,
      ),

      textAlign: TextAlign.center,
      decoration: InputDecoration(
        hintText: widget.hintText,
        hintStyle: const TextStyle(
          color: Palette.gray,
          fontSize: 15,
          fontWeight: FontWeight.w200,
        ),
        enabledBorder: UnderlineInputBorder(borderSide: BorderSide(color: Palette.gray)),
        focusedBorder: UnderlineInputBorder(borderSide: BorderSide(color: Palette.black)),
        filled: true,
        fillColor: Palette.white,
        isDense: true,
        contentPadding: EdgeInsets.fromLTRB(5,15,0,5)
      ),
      
      onChanged: (value){
        widget.controller.textFieldChanged(widget.type, value);
      },
      
      onSubmitted: (_){
        if(widget.nextEvent){
        }else{
          if(widget.sign){
            widget.controller.signIn('SignIn');
          }else{
            widget.controller.signUp('SignUp');
          }
        }
      },
    );
  }
}
```

</details>

<br>
 

---


 
## # 코드 설명 

<p>
asd
</p>


###
 
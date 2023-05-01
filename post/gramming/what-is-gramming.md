# What is 'Gramming'?

* **'Gramming'은?**
  * 베이킹할 때 중량 계산, 분할 등에 도움을 주는 앱입니다.
  * 메뉴 - 레시피 의 구조로 구성되어 있습니다
  * 각 레시피를 읽기, 작성, 수정, 삭제할 수 있습니다
  * 각 레시피에 기록된 중량을 곱하기, 나누기 연산하여 직관적으로 확인할 수 있습니다
  * 간단한 메모를 작성할 수 있습니다
  * 한글과 영어 양 언어로 제공됩니다



<이미지>





*   기술 스택

    * Flutter
    * Firebase


* **페이지 구조**

```
- main.dart (앱의 진입점(entry point) 역할을 합니다.)
  | 
  |- sign.dart (로그인/로그아웃 페이지입니다.)
  | 
  |- menu.dart (메뉴 페이지입니다, 연관성 있는 레시피들을 그룹화하는 역할을 합니다.)
      | 
      |- recipe.dart (레시피 화면입니다, 레시피를 보여주고 추가, 삭제, 수정할 수 있는 기능들을 제공합니다))
          | 
          |- add_recipe.dart (레시피 추가 페이지입니다.)
          | 
          |- edit_recipe.dart (레시피 수정 페이지입니다.)
```



* **Firebase DB 구조**

```dart
// (C) : Collection
// (D) : Document
// (F) : Field

- (C) user
  |
  |- (D) userUid
      |
      |- (F) menuList = [menu1, menu2, menu3, menu4]
      |   |
      |   |- (C) menu1
      |   |   |
      |   |   |- (D) memo = {
      |   |   |        Memo: "Memo Example..."
      |   |   |      }
      |   |   |
      |   |   |- (D) recipeList = [
      |   |   |        recipe1, recipe2, recipe3, recipe4
      |   |   |      ]
      |   |   |
      |   |   |- (D) recipe
      |   |       |
      |   |       |- (F) recipe1 = {
      |   |       |          divideWeight : 1
      |   |       |          multipleValue : 1
      |   |       |          ingredient = [ingredient1, ingredient2, ingredient3, ...]
      |   |       |          weight = [weight1, weight2, weight3, ...]
      |   |       |       }
      |   |       |
      |   |       |  ...
      |   |
      |   |- (C)menu2
      |   |   | ...
      |   |- (C)menu3
      |   |   | ...
      |   |- (C)menu4
      |       | ...
      | ...
      


```

```
- 디자인 
```

```
```

```
- 개발
```

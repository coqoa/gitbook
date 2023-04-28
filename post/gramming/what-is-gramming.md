# What is 'Gramming'?

* **What is 'Gramming'?**
  * 베이킹 계량, 분할 등 중량 계산에 도움을 주고, 간단한 메모를 작성할 수있는 앱입니다.
  * 메뉴 - 레시피 의 구조로 구성되어 있습니다
  * 각 레시피를 읽기, 작성, 수정, 삭제할 수 있습니다
  * 각 레시피에 작성된 중량을 곱하기, 나누기 연산을 할 수 있고 직관적으로 확인할 수 있습니다ㄱ
  * 간단한 메모를 작성할 수 있습니다
  * 한글 / 영어 두가지 언어로 제공합니다



<이미지>





*   Tech Stack

    * Flutter
    * Firebase


* Page Structure

```
{
    main.dart
        sign.dart
        menu.dart
            recipe.dart
                add_recipe.dart
                edit_recipe.dart
}
```



* Database Structure

```dart
{
    user
        userUid
            menuList = [menu1, menu2, menu3, menu4]
            menu1
                memo = {Memo:""}
                recipeList = [recipe1, recipe2, recipe3, recipe4]
                recipe
                    recipe1 = {
                        divideWeight : ???
                        multipleValue : ???
                        ingredient = [ing1, ing2, ing3 ...]
                        weight = [wei1, wei2, wei3 ...]
                    }
    ...

}
```

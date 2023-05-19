```dart
// Get

Future<void> getTagList(String tag) async{
    // 문자열 'tag'를 param으로 넣어서 통신한 결과를 model을 통해 List로 만들어주는 작업을 하고 List가 있다면 해쉬태그 페이지를 열어주는 역할을 한다
}
```

```dart

// Validation

  void validationHashTag(String v, inputController) async{
    int defaultSharpIndex = v.indexOf('#');
    late int sharpIndex = -1;
    List sharpIndexList = [];

    // # 커서위치 = _inputController.selection.base.offset

    // 2-1. List에 해쉬태그의 위치 index 추가하기
    while(defaultSharpIndex != -1){
      sharpIndexList.add(defaultSharpIndex);
      defaultSharpIndex = v.indexOf('#', defaultSharpIndex+'#'.length);
    }

    // 2-2. 커서에서 가장 가까운 # 인덱스 구하기
    for(int i=0; i<sharpIndexList.length; i++){
      if(sharpIndexList[i] < inputController.selection.base.offset){
        sharpIndex = sharpIndexList[i];
      }
    }

    // 2-3. # ~ 커서까지의 값을 hashTagResult에 할당
    if(v.contains('#') ){
      if(sharpIndex != -1){
        hashTagResult.value = v.substring(sharpIndex, inputController.selection.base.offset);
      }
    }else{
      hashTagResult.value = '';
    }

    // 2-4. hashTagResult 체크해서 getTagList() 호출
    if(hashTagResult.value.isNotEmpty){
      if(hashTagResult.contains('\n')){
        // 줄넘김 있음
        hashtagPageOn.value = false;
      }else{
        // 줄넘김 없음
        getTagList(hashTagResult.value);
      }
    }
  }
```
#### validationHashTag( ) 
- 문자열 `v`와 `inputController`를 통해 `#`의 위치를 정하고 `해당 # ~ 커서`까지의 문자열을 `getTagList()`의 파라미터로 넘기는 역할을 한다
#### 변수
- `defaultSharpIndex` : param으로 전달받은 문자열 `v`에 있는 첫 번째 `#` 인덱스를 저장
- `sharpIndex` : 기준이 되는 `#` 인덱스
- `sharpIndexList` : `#` 인덱스를 담아두는 리스트
#### 함수
- 2-1 : `v`에 `#`이 있으면 while문을 통해 `#`의 위치 index를 리스트에 담아준 뒤, `defaultSharpIndex`를 다음 `#`의 인덱스로 갱신한다
- 2-2 : `sharpIndexList`의 크기만큼 for문 반복해서 커서에서 가장 가까운 `#`의 인덱스를 구해서 `sharpIndex`에 할당한다
- 2-3 : `#`이 포함된 문자열이고 `sharpIndex`이 -1이 아니라면 `sharpIndex ~ 커서까지의 값`을 `hashTagResult`에 할당한다
- 2-4 : `hashTagResult`에 줄넘김이 있는지 체크하고 없다면 `getTagList()`의 인자로 넣어서 호출
<br>
```dart
// Replace 

  void replaceHashtag(String v, inputController, replaceString) async{
    int defaultSharpIndex = v.indexOf('#');
    late int sharpIndex = -1;
    List sharpIndexList = [];

    //  # 커서위치 = _inputController.selection.base.offset

    while(defaultSharpIndex!=-1){
      sharpIndexList.add(defaultSharpIndex);
      defaultSharpIndex = v.indexOf('#', defaultSharpIndex+'#'.length);
    }

    for(int i=0; i<sharpIndexList.length; i++){
      if(sharpIndexList[i] < inputController.selection.base.offset){
        sharpIndex = sharpIndexList[i];
      }
    }

    String newText = '';
    // 2-1. 커서가 끝이면 그냥추가
    if(inputController.selection.base.offset == inputController.text.length){
      newText = inputController.text.replaceRange(sharpIndex,inputController.selection.base.offset,replaceString);
    }else{
      if(inputController.text[inputController.selection.base.offset] == ' '){
        // 2-2. 커서가 중간에 있는데 공백이 있으면 ? 그냥추가
        newText = inputController.text.replaceRange(sharpIndex,inputController.selection.base.offset,replaceString);
      }else{
        // 2-3. 커서가 중간인데 뒤에 공백이 없으면 ? 공백추가
        newText = inputController.text.replaceRange(sharpIndex,inputController.selection.base.offset,replaceString+' ');
      }
    }
    
    // 2-4. 실제 값 변환, 커서 이동, hashtagPage 닫아주기
    inputController.value = TextEditingValue(
      text: newText,
      selection: TextSelection.collapsed(offset: newText.length) // 수정 후 커서 위치
    );
    hashtagPageOn.value = false;
  }
```
#### replaceHashtag()
- getTagList()를 통해 불러온 리스트중 하나를 클릭했을 때 현재 커서에서 가장 가까운 #을 변경하는 메소드
#### 함수
- `String newText = '';` 위 까지는 validationHashTag()와 동일하다
- replaceHashtag()와 다른점 : getTagList()메소드를 빼고 커서의 위치에따라 공백을 추가하는 로직이 들어간다
  - 2-1. 커서가 끝에 있으면 그냥 변환
  - 2-2. 커서가 중간에 있는데 공백이 있으면 그냥 변환
  - 2-3. 커서가 중간에 있고 공백이 없으면 공백 추가해서 변환 
  - 2-4. 변환 된 텍스트를 inputController에 넣어주고 커서 위치를 설정해준다음 hashtagPageOn를 닫아준다
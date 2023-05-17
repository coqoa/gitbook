# Google Place API

### 프로젝트 설정
1. [Google Cloud](https://console.cloud.google.com/) 접속, 로그인
2. 새 프로젝트를 만들거나 기존 프로젝트 선택
3. 상단 검색창에 'API 및 서비스'검색후 해당 메뉴로 이동
4. API 및 서비스 메뉴 상단 `+ API 및 서비스 사용설정`버튼  클릭 > 'Places API'를 검색해서 **활성화**
5. API 및 서비스 메뉴 좌측 매뉴 '사용자 인증 정보'로 이동해서 API키 생성 (사용자 인증정보 만들기 - API 키)

### 플러터 프로젝트에 주소 검색 메소드 추가 
> http 요청을 통해 textSearch 구현하기
```dart 
// Controller

class Controller extends GetxController{
    RxList<AddressSearchModel> addressList = <AddressSearchModel>[].obs;

    Future<void> getAddressSearch(String text) async{

        List<AddressSearchModel> temporaryList = <AddressSearchModel>[];

        final url = Uri.parse(
            'https://maps.googleapis.com/maps/api/place/textsearch/json?query=$text&language=$deviceLocale&key=$APIKEY'
        ); 

        final response = await http.get(url);
        
        var decoded = await jsonDecode(utf8.decode(response.bodyBytes));

        if(decoded['error_message']==null){
            for(var i in decoded['results']){
                AddressSearchModel model = AddressSearchModel.fromJson(i);
                temporaryList.add(model);
            }
        }

        addressList.value = temporaryList;
    }
}
```
```dart
// Model 

class AddressSearchModel {
    String? name;
    String? formattedAddress;
    double? lat;
    double? lng;

    AddressSearchModel.empty() {
        name = null;
        formattedAddress = null;
        lat = null;
        lng = null;
    }

    AddressSearchModel.fromJson(Map<String, dynamic> data) {
        name = data['name']??"";
        formattedAddress = data['formatted_address']??"";
        lat = data['geometry']['location']['lat']??"0.0000000";
        lng = data['geometry']['location']['lng']??"0.0000000";
    }
}
```

1. `temporaryList`는 for문을 통해 생성된 데이터를 하나씩 담아서 `addressList`에 한번에 넣기 위한 변수
2. `Uri.parse()`함수를 사용해서 URL을 Uri객체로 파싱하고 이를 `http.get()`의 파라미터로 전달한다  
여기서 `$text`, `$deviceLocale`, `$APIKEY` 변수들은 실제 값으로 대체되어야 함
1. 응답 결과인 response는 `jsonDecode()`함수를 통해 JSON 데이터를 디코딩 하는 과정이 필요하다
2. 파라미터로 response가 아닌 `utf8.decode(response.bodyBytes)`를 넣는 이유는 한글 깨짐 현상을 방지하기 위함이다
3. 변환된 데이터인 `decoded`를 for문을 사용해서 반복적으로 처리하고, `model.fromJson()`메소드를 통해 직렬화하여 얻은 결과를 `temporaryList`변수에 하나씩 넣는다
4. for문 작업이 완료되면 `temporaryList`를 `addressList`에 할당한다
5. 이 작업이 완료된 후에는 `controller.addressList[index].name`과 같은 형태로 사용할 수 있다.
# [JSON 직렬화](https://flutter-ko.dev/development/data-and-backend/json)

> 외부 api를 사용하거나 서버를 통해 받은 데이터를 편하게 사용하기 위한 작업
 
<br>
 
## # 데이터

- 다음 예제 데이터는 `google place` api를 이용해 '중앙보훈병원'을 검색한 뒤 응답받은 JSON 데이터다
    <details>
    <summary>예제 데이터 전체 코드</summary>

    ```
    {
        "html_attributions" : [],
        "results" : [
            {
                "business_status" : "OPERATIONAL",
                "formatted_address" : "\ub300\ud55c\ubbfc\uad6d \uc11c\uc6b8\ud2b9\ubcc4\uc2dc \uac15\ub3d9\uad6c \ub454\ucd0c\ub3d9 6-2",
                "geometry" : {
                    "location" : {
                    "lat" : 37.5309362,
                    "lng" : 127.1483746
                    },
                    "viewport" : {
                    "northeast" : {
                        "lat" : 37.53228602989272,
                        "lng" : 127.1497244298927
                    },
                    "southwest" : {
                        "lat" : 37.52958637010728,
                        "lng" : 127.1470247701073
                    }
                    }
                },
                "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/v1/png_71/generic_business-71.png",
                "icon_background_color" : "#7B9EB0",
                "icon_mask_base_uri" : "https://maps.gstatic.com/mapfiles/place_api/icons/v2/generic_pinlet",
                "name" : "\uc911\uc559\ubcf4\ud6c8\ubcd1\uc6d0 \uc751\uae09\uc2e4",
                "opening_hours" : {
                    "open_now" : true
                },
                "place_id" : "ChIJzd4ZLN6vfDUR_Sr5VgfWVUg",
                "plus_code" : {
                    "compound_code" : "G4JX+98 \uc11c\uc6b8\ud2b9\ubcc4\uc2dc",
                    "global_code" : "8Q99G4JX+98"
                },
                "rating" : 4,
                "reference" : "ChIJzd4ZLN6vfDUR_Sr5VgfWVUg",
                "types" : [ "health", "point_of_interest", "establishment" ],
                "user_ratings_total" : 1
            },
            {
                "business_status" : "OPERATIONAL",
                "formatted_address" : "\ub300\ud55c\ubbfc\uad6d \uc11c\uc6b8\ud2b9\ubcc4\uc2dc",
                "geometry" : {
                    "location" : {
                    "lat" : 37.5268002,
                    "lng" : 127.1476326
                    },
                    "viewport" : {
                    "northeast" : {
                        "lat" : 37.52815002989271,
                        "lng" : 127.1489824298927
                    },
                    "southwest" : {
                        "lat" : 37.52545037010727,
                        "lng" : 127.1462827701073
                    }
                    }
                },
                "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/v1/png_71/generic_business-71.png",
                "icon_background_color" : "#7B9EB0",
                "icon_mask_base_uri" : "https://maps.gstatic.com/mapfiles/place_api/icons/v2/generic_pinlet",
                "name" : "\uc911\uc559\ubcf4\ud6c8\ubcd1\uc6d0\uc5ed1\ubc88\ucd9c\uad6c",
                "photos" : [
                    {
                    "height" : 1800,
                    "html_attributions" : [
                        "\u003ca href=\"https://maps.google.com/maps/contrib/109451593717323116772\"\u003eA Google User\u003c/a\u003e"
                    ],
                    "photo_reference" : "AZose0mdqQ-8HByoaoRnDlQTNaXm3J67EfnZ66uxbo-ElhGKiM2x0YyP8s8Zv9_iT7p5ET0NL5Bi4sPqh3tK5GdnM5_UAgetCLW6w-jTCpoLdLADxrP9DjRUJKALCXItv0QCweNg3lLlLN6mU9XDq76l-wSMqRtvdLfSv-tYxGka4nRsxkK0",
                    "width" : 4000
                    }
                ],
                "place_id" : "ChIJe8SV-92vfDURuHwOUIR7fnk",
                "plus_code" : {
                    "compound_code" : "G4GX+P3 \uc11c\uc6b8\ud2b9\ubcc4\uc2dc",
                    "global_code" : "8Q99G4GX+P3"
                },
                "rating" : 4.6,
                "reference" : "ChIJe8SV-92vfDURuHwOUIR7fnk",
                "types" : [ "transit_station", "point_of_interest", "establishment" ],
                "user_ratings_total" : 9
            },
            {
                "business_status" : "OPERATIONAL",
                "formatted_address" : "\ub300\ud55c\ubbfc\uad6d \uc11c\uc6b8\ud2b9\ubcc4\uc2dc",
                "geometry" : {
                    "location" : {
                    "lat" : 37.527747,
                    "lng" : 127.147685
                    },
                    "viewport" : {
                    "northeast" : {
                        "lat" : 37.52909682989272,
                        "lng" : 127.1490348298927
                    },
                    "southwest" : {
                        "lat" : 37.52639717010727,
                        "lng" : 127.1463351701072
                    }
                    }
                },
                "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/v1/png_71/generic_business-71.png",
                "icon_background_color" : "#7B9EB0",
                "icon_mask_base_uri" : "https://maps.gstatic.com/mapfiles/place_api/icons/v2/generic_pinlet",
                "name" : "9\ud638\uc120\uc911\uc559\ubcf4\ud6c8\ubcd1\uc6d0\uc5ed1\ubc88\ucd9c\uad6c",
                "photos" : [
                    {
                    "height" : 4032,
                    "html_attributions" : [
                        "\u003ca href=\"https://maps.google.com/maps/contrib/100862419511529207617\"\u003eA Google User\u003c/a\u003e"
                    ],
                    "photo_reference" : "AZose0mwJ1ap2t8P49kHdzb262OfVTlqXTCz56lIOT4nQaGRPKFi3iIfEYewfLXese_jxpMuvE5Oy8F-Jc_CDNuOftTgKsPknl4Rkfsr6cz1MaD6YxOYdLeqvHG1g6NvwDjgHIId59s70nybIW-7040HKHIpYqVL8ZC5jtrSnU6fgASAyfAl",
                    "width" : 3024
                    }
                ],
                "place_id" : "ChIJN0jaBt6vfDURvNl-JwIEPKM",
                "plus_code" : {
                    "compound_code" : "G4HX+33 \uc11c\uc6b8\ud2b9\ubcc4\uc2dc",
                    "global_code" : "8Q99G4HX+33"
                },
                "rating" : 2.8,
                "reference" : "ChIJN0jaBt6vfDURvNl-JwIEPKM",
                "types" : [ "transit_station", "point_of_interest", "establishment" ],
                "user_ratings_total" : 5
            },
            {
                "business_status" : "OPERATIONAL",
                "formatted_address" : "\ub300\ud55c\ubbfc\uad6d \uc11c\uc6b8\ud2b9\ubcc4\uc2dc \uac15\ub3d9\uad6c \uc9c4\ud669\ub3c4\ub85c61\uae38 53",
                "geometry" : {
                    "location" : {
                    "lat" : 37.5281779,
                    "lng" : 127.1475226
                    },
                    "viewport" : {
                    "northeast" : {
                        "lat" : 37.52952772989272,
                        "lng" : 127.1488724298927
                    },
                    "southwest" : {
                        "lat" : 37.52682807010728,
                        "lng" : 127.1461727701073
                    }
                    }
                },
                "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/v1/png_71/generic_business-71.png",
                "icon_background_color" : "#7B9EB0",
                "icon_mask_base_uri" : "https://maps.gstatic.com/mapfiles/place_api/icons/v2/generic_pinlet",
                "name" : "\uc911\uc559\ubcf4\ud6c8\ubcd1\uc6d0 \uc7a5\ub840\uc2dd\uc7a5",
                "opening_hours" : {
                    "open_now" : true
                },
                "photos" : [
                    {
                    "height" : 576,
                    "html_attributions" : [
                        "\u003ca href=\"https://maps.google.com/maps/contrib/104977657339461618435\"\u003eA Google User\u003c/a\u003e"
                    ],
                    "photo_reference" : "AZose0msfNATuTMBGHZ2Hobcc_iY6n-lvyrBBjilOxCLJHyioO-elWpMIyVeyd-ycE0zvLzIU8dclLrQHfcEmYpfkCU-nZ_8HVwkCklRFfUJGEqcKBGrJFavUqIiBG3c64mLXMYL_-WkJg8guB5Mh_UjemY5vkKj1ZhfuEfTXW0MiN-i_bYh",
                    "width" : 1024
                    }
                ],
                "place_id" : "ChIJm9eixN-vfDURx9JwziQWrd8",
                "plus_code" : {
                    "compound_code" : "G4HX+72 \uc11c\uc6b8\ud2b9\ubcc4\uc2dc",
                    "global_code" : "8Q99G4HX+72"
                },
                "rating" : 3.8,
                "reference" : "ChIJm9eixN-vfDURx9JwziQWrd8",
                "types" : [ "funeral_home", "point_of_interest", "establishment" ],
                "user_ratings_total" : 5
            },
            {
                "business_status" : "OPERATIONAL",
                "formatted_address" : "\ub300\ud55c\ubbfc\uad6d \uc11c\uc6b8\ud2b9\ubcc4\uc2dc",
                "geometry" : {
                    "location" : {
                    "lat" : 37.529458,
                    "lng" : 127.148983
                    },
                    "viewport" : {
                    "northeast" : {
                        "lat" : 37.53080782989272,
                        "lng" : 127.1503328298927
                    },
                    "southwest" : {
                        "lat" : 37.52810817010728,
                        "lng" : 127.1476331701073
                    }
                    }
                },
                "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/v1/png_71/generic_business-71.png",
                "icon_background_color" : "#7B9EB0",
                "icon_mask_base_uri" : "https://maps.gstatic.com/mapfiles/place_api/icons/v2/generic_pinlet",
                "name" : "\uc911\uc559\ubcf4\ud6c8\ubcd1\uc6d0\uc5ed3\ubc88\ucd9c\uad6c",
                "photos" : [
                    {
                    "height" : 3024,
                    "html_attributions" : [
                        "\u003ca href=\"https://maps.google.com/maps/contrib/116372416429316556811\"\u003ehyunhana\u003c/a\u003e"
                    ],
                    "photo_reference" : "AZose0lNV_W1tH6hCAg9ndZEhvkE22y2_GRvie8Qm50F2rYxcwbyVHONfb7p58MUGm_hqrr63YAQVxF-4RZnWdqdnkb416IhZeGth1Mrri7HQy-QVygw6gcjWm1NUNFXLx-12kmff11byPnDKcKm6rxDWWAf9selEnLKpujfK_zrcvqPyHsn",
                    "width" : 4032
                    }
                ],
                "place_id" : "ChIJg7DZS96vfDURRdTXSE9AzdQ",
                "plus_code" : {
                    "compound_code" : "G4HX+QH \uc11c\uc6b8\ud2b9\ubcc4\uc2dc",
                    "global_code" : "8Q99G4HX+QH"
                },
                "rating" : 4.3,
                "reference" : "ChIJg7DZS96vfDURRdTXSE9AzdQ",
                "types" : [ "transit_station", "point_of_interest", "establishment" ],
                "user_ratings_total" : 3
            }
        ],
        "status" : "OK"
    }
    ```
    </details>
    <br>

- 이를 [여기](https://freeonlinetools24.com/json-decode)에서 decode 해보면 다음과 같은 데이터가 나온다
    <details>
    <summary>decode 완료 데이터</summary>

    ```
    array (
        'html_attributions' => 
        array (
        ),
        'results' => 
        array (
            0 => 
            array (
            'business_status' => 'OPERATIONAL',
            'formatted_address' => '대한민국 서울특별시 강동구 둔촌동 6-2',
            'geometry' => 
            array (
                'location' => 
                array (
                'lat' => 37.5309362,
                'lng' => 127.1483746,
                ),
                'viewport' => 
                array (
                'northeast' => 
                array (
                    'lat' => 37.53228602989272,
                    'lng' => 127.1497244298927,
                ),
                'southwest' => 
                array (
                    'lat' => 37.52958637010728,
                    'lng' => 127.1470247701073,
                ),
                ),
            ),
            'icon' => 'https://maps.gstatic.com/mapfiles/place_api/icons/v1/png_71/generic_business-71.png',
            'icon_background_color' => '#7B9EB0',
            'icon_mask_base_uri' => 'https://maps.gstatic.com/mapfiles/place_api/icons/v2/generic_pinlet',
            'name' => '중앙보훈병원 응급실',
            'opening_hours' => 
            array (
                'open_now' => true,
            ),
            'place_id' => 'ChIJzd4ZLN6vfDUR_Sr5VgfWVUg',
            'plus_code' => 
            array (
                'compound_code' => 'G4JX+98 서울특별시',
                'global_code' => '8Q99G4JX+98',
            ),
            'rating' => 4,
            'reference' => 'ChIJzd4ZLN6vfDUR_Sr5VgfWVUg',
            'types' => 
            array (
                0 => 'health',
                1 => 'point_of_interest',
                2 => 'establishment',
            ),
            'user_ratings_total' => 1,
            ),
            1 => 
            array (
            'business_status' => 'OPERATIONAL',
            'formatted_address' => '대한민국 서울특별시',
            'geometry' => 
            array (
                'location' => 
                array (
                'lat' => 37.5268002,
                'lng' => 127.1476326,
                ),
                'viewport' => 
                array (
                'northeast' => 
                array (
                    'lat' => 37.52815002989271,
                    'lng' => 127.1489824298927,
                ),
                'southwest' => 
                array (
                    'lat' => 37.52545037010727,
                    'lng' => 127.1462827701073,
                ),
                ),
            ),
            'icon' => 'https://maps.gstatic.com/mapfiles/place_api/icons/v1/png_71/generic_business-71.png',
            'icon_background_color' => '#7B9EB0',
            'icon_mask_base_uri' => 'https://maps.gstatic.com/mapfiles/place_api/icons/v2/generic_pinlet',
            'name' => '중앙보훈병원역1번출구',
            'photos' => 
            array (
                0 => 
                array (
                'height' => 1800,
                'html_attributions' => 
                array (
                    0 => 'A Google User',
                ),
                'photo_reference' => 'AZose0mdqQ-8HByoaoRnDlQTNaXm3J67EfnZ66uxbo-ElhGKiM2x0YyP8s8Zv9_iT7p5ET0NL5Bi4sPqh3tK5GdnM5_UAgetCLW6w-jTCpoLdLADxrP9DjRUJKALCXItv0QCweNg3lLlLN6mU9XDq76l-wSMqRtvdLfSv-tYxGka4nRsxkK0',
                'width' => 4000,
                ),
            ),
            'place_id' => 'ChIJe8SV-92vfDURuHwOUIR7fnk',
            'plus_code' => 
            array (
                'compound_code' => 'G4GX+P3 서울특별시',
                'global_code' => '8Q99G4GX+P3',
            ),
            'rating' => 4.6,
            'reference' => 'ChIJe8SV-92vfDURuHwOUIR7fnk',
            'types' => 
            array (
                0 => 'transit_station',
                1 => 'point_of_interest',
                2 => 'establishment',
            ),
            'user_ratings_total' => 9,
            ),
            2 => 
            array (
            'business_status' => 'OPERATIONAL',
            'formatted_address' => '대한민국 서울특별시',
            'geometry' => 
            array (
                'location' => 
                array (
                'lat' => 37.527747,
                'lng' => 127.147685,
                ),
                'viewport' => 
                array (
                'northeast' => 
                array (
                    'lat' => 37.52909682989272,
                    'lng' => 127.1490348298927,
                ),
                'southwest' => 
                array (
                    'lat' => 37.52639717010727,
                    'lng' => 127.1463351701072,
                ),
                ),
            ),
            'icon' => 'https://maps.gstatic.com/mapfiles/place_api/icons/v1/png_71/generic_business-71.png',
            'icon_background_color' => '#7B9EB0',
            'icon_mask_base_uri' => 'https://maps.gstatic.com/mapfiles/place_api/icons/v2/generic_pinlet',
            'name' => '9호선중앙보훈병원역1번출구',
            'photos' => 
            array (
                0 => 
                array (
                'height' => 4032,
                'html_attributions' => 
                array (
                    0 => 'A Google User',
                ),
                'photo_reference' => 'AZose0mwJ1ap2t8P49kHdzb262OfVTlqXTCz56lIOT4nQaGRPKFi3iIfEYewfLXese_jxpMuvE5Oy8F-Jc_CDNuOftTgKsPknl4Rkfsr6cz1MaD6YxOYdLeqvHG1g6NvwDjgHIId59s70nybIW-7040HKHIpYqVL8ZC5jtrSnU6fgASAyfAl',
                'width' => 3024,
                ),
            ),
            'place_id' => 'ChIJN0jaBt6vfDURvNl-JwIEPKM',
            'plus_code' => 
            array (
                'compound_code' => 'G4HX+33 서울특별시',
                'global_code' => '8Q99G4HX+33',
            ),
            'rating' => 2.8,
            'reference' => 'ChIJN0jaBt6vfDURvNl-JwIEPKM',
            'types' => 
            array (
                0 => 'transit_station',
                1 => 'point_of_interest',
                2 => 'establishment',
            ),
            'user_ratings_total' => 5,
            ),
            3 => 
            array (
            'business_status' => 'OPERATIONAL',
            'formatted_address' => '대한민국 서울특별시 강동구 진황도로61길 53',
            'geometry' => 
            array (
                'location' => 
                array (
                'lat' => 37.5281779,
                'lng' => 127.1475226,
                ),
                'viewport' => 
                array (
                'northeast' => 
                array (
                    'lat' => 37.52952772989272,
                    'lng' => 127.1488724298927,
                ),
                'southwest' => 
                array (
                    'lat' => 37.52682807010728,
                    'lng' => 127.1461727701073,
                ),
                ),
            ),
            'icon' => 'https://maps.gstatic.com/mapfiles/place_api/icons/v1/png_71/generic_business-71.png',
            'icon_background_color' => '#7B9EB0',
            'icon_mask_base_uri' => 'https://maps.gstatic.com/mapfiles/place_api/icons/v2/generic_pinlet',
            'name' => '중앙보훈병원 장례식장',
            'opening_hours' => 
            array (
                'open_now' => true,
            ),
            'photos' => 
            array (
                0 => 
                array (
                'height' => 576,
                'html_attributions' => 
                array (
                    0 => 'A Google User',
                ),
                'photo_reference' => 'AZose0msfNATuTMBGHZ2Hobcc_iY6n-lvyrBBjilOxCLJHyioO-elWpMIyVeyd-ycE0zvLzIU8dclLrQHfcEmYpfkCU-nZ_8HVwkCklRFfUJGEqcKBGrJFavUqIiBG3c64mLXMYL_-WkJg8guB5Mh_UjemY5vkKj1ZhfuEfTXW0MiN-i_bYh',
                'width' => 1024,
                ),
            ),
            'place_id' => 'ChIJm9eixN-vfDURx9JwziQWrd8',
            'plus_code' => 
            array (
                'compound_code' => 'G4HX+72 서울특별시',
                'global_code' => '8Q99G4HX+72',
            ),
            'rating' => 3.8,
            'reference' => 'ChIJm9eixN-vfDURx9JwziQWrd8',
            'types' => 
            array (
                0 => 'funeral_home',
                1 => 'point_of_interest',
                2 => 'establishment',
            ),
            'user_ratings_total' => 5,
            ),
            4 => 
            array (
            'business_status' => 'OPERATIONAL',
            'formatted_address' => '대한민국 서울특별시',
            'geometry' => 
            array (
                'location' => 
                array (
                'lat' => 37.529458,
                'lng' => 127.148983,
                ),
                'viewport' => 
                array (
                'northeast' => 
                array (
                    'lat' => 37.53080782989272,
                    'lng' => 127.1503328298927,
                ),
                'southwest' => 
                array (
                    'lat' => 37.52810817010728,
                    'lng' => 127.1476331701073,
                ),
                ),
            ),
            'icon' => 'https://maps.gstatic.com/mapfiles/place_api/icons/v1/png_71/generic_business-71.png',
            'icon_background_color' => '#7B9EB0',
            'icon_mask_base_uri' => 'https://maps.gstatic.com/mapfiles/place_api/icons/v2/generic_pinlet',
            'name' => '중앙보훈병원역3번출구',
            'photos' => 
            array (
                0 => 
                array (
                'height' => 3024,
                'html_attributions' => 
                array (
                    0 => 'hyunhana',
                ),
                'photo_reference' => 'AZose0lNV_W1tH6hCAg9ndZEhvkE22y2_GRvie8Qm50F2rYxcwbyVHONfb7p58MUGm_hqrr63YAQVxF-4RZnWdqdnkb416IhZeGth1Mrri7HQy-QVygw6gcjWm1NUNFXLx-12kmff11byPnDKcKm6rxDWWAf9selEnLKpujfK_zrcvqPyHsn',
                'width' => 4032,
                ),
            ),
            'place_id' => 'ChIJg7DZS96vfDURRdTXSE9AzdQ',
            'plus_code' => 
            array (
                'compound_code' => 'G4HX+QH 서울특별시',
                'global_code' => '8Q99G4HX+QH',
            ),
            'rating' => 4.3,
            'reference' => 'ChIJg7DZS96vfDURRdTXSE9AzdQ',
            'types' => 
            array (
                0 => 'transit_station',
                1 => 'point_of_interest',
                2 => 'establishment',
            ),
            'user_ratings_total' => 3,
            ),
        ),
        'status' => 'OK',
    )
    ```
    </details>
    <br>
- 필요한 데이터는 `name`, `formatted_address`, `lat`, `lng`

<br>

## # `model.dart` 생성
 
<br>
 
```dart

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

## # 응답 데이터`(response)`를 for문을 통해 model에 넣어주고 결과를 리스트에 추가하는 작업
 
<br>
 
```dart
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

- model을 경유하지 않았을 때 데이터 사용법
 
    <br>
    
    ```dart
    controller.decoded['results'][index]['name'];
    controller.decoded['results'][index]['formatted_address'];
    controller.decoded['results'][index]['geometry']['location']['lat'];
    controller.decoded['results'][index]['geometry']['location']['lng'];
    ```
- model을 경유해서 만들어진 데이터 사용법
 
    <br>
    
    ```dart
    controller.addressList[index].name;
    controller.addressList[index].formattedAddress;
    controller.addressList[index].lat;
    controller.addressList[index].lat;
    ```
## # 결과 
 
<br>

- 데이터를 사용할 때마다 JSON 데이터의 구조를 확인할 필요가 없어진다
  
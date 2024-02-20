---
layout: single
title: "📘[JavaScript] JS에서 데이터를 동적으로 가져오기"
toc: true
toc_sticky: true
toc_label: "목차"
categories: javascript
excerpt: ""
tag: [web]
---

# 데이터를 화면에 바로 표시하는 법(?)
jsp 화면을 구현하는 도중 문제가 생겼다.  
화면에서 `SB Grid`를 클릭했을 때, 해당 셀의 데이터를 파싱하여 화면에 표시해야하는 작업이었다.  
- SB Grid란, 현재 실습 과정에서 사용하고 있는 데이터 시각화 도구로, 데이터를 엑셀과 같은 형태로 화면에 나타낼 수 있으며, 다양한 기능들이 있다.  

먼저, 데이터를 클릭했을 때 그 셀의 데이터를 파싱하는 법은 SB Grid 내장 함수를 이용하여 아래와 같다.  
```
var nRow = datagrid.getRow();
var selectData = datagrid.getCellData(nRow, 2);
```  

여기서 `getCellData(nRow, 2)`는 클릭한 셀 데이터 중 2번째 열에 해당하는 데이터를 파싱하겠다는 의미이다.  

[예시]  
```
CellData: |projectSeq|ProjectCode|ProjectValue|ProjectStartDate|ProjectEndDate|
>>> getCellData(nRow, 2): ProjectCode에 해당하는 값을 가져옴
```  

이어서 Ajax를 이용해 컨트롤러에 `selectData`를 전달하고, 백엔드 로직(Controller -> Service -> ServiceImpl -> Mapper -> DB)을 거쳐 데이터를 조회했다고 하자.  
그러면 그 데이터는 어떻게 표현하냐?!  
```
$.ajax({
        url: "<c:url value='/location/to/controller'/>",
        type: "POST",
        data: pageInfo,
        dataType: 'json',
        async: false,
        success: function (data) {
            if (data != null && data.length > 0) {
                var orderData = data[0]; 

                var data1 = orderData.projectCode;

                document.getElementById("<html에서 값을 넣을 공간의 id 값>").value = data1;

            } else {
                alert("Failed to fetch data");
            }
        }
    });
```  
위와 같이 `data[0]`로 표현할 수 있다.  
data[0]에는 내가 선택한 셀 데이터의 2번째 데이터인 `ProjectCode`를 이용해 쿼리를 동작한 후 SELECT한 데이터의 전체 값이 들어있다.  
이 값을 이용해 `orderData`에 넣어주어 위처럼 사용할 수 있다.  
이렇게 하면 화면에서 셀 데이터를 클릭하고, 해당 값을 화면의 이동 없이 백엔드 로직을 거쳐 화면에 표현할 수 있다.  

**여기서 사용한 SB Grid는 SBGrid2.5 이며, 현재는 SBGrid3.0 이 있다.**  
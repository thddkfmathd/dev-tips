##  click() vs on("click")

<br>

> 동일한 동작이지만 동적요소 까지 고려한다면 ***on("click")***
```javascript
// document.ready 내에서 정적요소를 대상으로 정의 
$("#ex").click(function(){...})

// document.ready 밖에서 동적요소까지 고려해 정의
$("#ex").on("click",function(){...})
```

<br>

> include 된 jsp파일에 document.ready 없음 

```javascript
// main.jsp

  <div>
    <button id="modalBtn"> 모달 </button>
    <%@include file="./exModal.jsp"%>
  </div>
  <script>
    $(document).ready(function () {
      $("#modalBtn").click(function(){
        $("#exModal").modal("show");
      }) 
    });
  </script>
```

```javascript
// exModal.jsp

  <div class="modal hide fade">
    <h5>모달</h5>
    <button id="alertBtn"> 알림 </button>
  </div>
  <script>
    $("#modalBtn").on("click",function(){
      alert('알림');
    })
  </script>
```
<br>



#### 정리
> 한 페이지 내에 document.ready 중복 정의하면서 .click으로 이벤트 바인딩 시 중복으로 바인딩될수있음

> document.ready 를 한번만 정의한다

> $('').on("click",function(){}) 을 쓴다

> 또는 이벤트를 바인딩하는 함수를 작성한다

```javascript
// include된 모달 열때 bindEvent() 를 호출한다던가

// 📌 1. `document.ready`는 한 번만 정의
$(document).ready(function () {
  $("#modalBtn").click(function(){
    $("#exModal").modal("show"); 
    bindEvent();  // 모달 열릴 때 이벤트 바인딩
  });
});

// 📌 2. 동적 요소 이벤트 바인딩 → `on("click")` 사용
function bindEvent(){
  // .off -> .on 중복 바인딩 방지
  $("#alertBtn").off("click").on("click", function(){
    alert('알림');
  });
}

```

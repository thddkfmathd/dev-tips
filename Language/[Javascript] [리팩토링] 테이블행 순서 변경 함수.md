### 행 순서변경 함수 리팩토링
- row_UP,row_DOWN 함수 내에서 중복되는 부분이 많음
- row_UP,row_DOWN 함수도 UP ,DOWN 여부를 파라미터로 받으면 한 함수로 될것 같았음 

> ###### 기존 source 
```
$('#row_DOWN').click(function(e){
	$('#saveChangeCheck').val("Y");
	
	if($("#testResultTable_wrapper").is(":visible")){
		var count = $('#testResultTable_wrapper .idx_radio:checked').parent().parent().find('.idx_row').val();
		//var a = $('.idx_radio:checked').parents('tr');
		var fullData = testResultTable.data().toArray();
		if(count == fullData.length){
			return false;
		}else{
			var arrItem = fullData[count-1]
			fullData[count-1] = fullData[count] 
			fullData[count] = arrItem;	
		}
		testResultTable.clear().draw()
		testResultTable.rows.add(fullData).draw(false);	
		$('#testResultTable_wrapper .idx_radio').eq(count).prop('checked', true);
	
	}else if($("#testLogTable_wrapper").is(":visible")){
		var count = $('#testLogTable_wrapper .idx_radio:checked').parent().parent().find('.idx_row').val();
		//var a = $('.idx_radio:checked').parents('tr');
		var fullData = testLogTable.data().toArray();
		if(count == fullData.length){
			return false;
		}else{
			var arrItem = fullData[count-1]
			fullData[count-1] = fullData[count] 
			fullData[count] = arrItem;	
		}
		testLogTable.clear().draw()
		testLogTable.rows.add(fullData).draw(false);	
		$('#testLogTable_wrapper .idx_radio').eq(count).prop('checked', true);
	}else if($("#testCaseTable_wrapper").is(":visible")){
		var count = $('#testCaseTable_wrapper .idx_radio:checked').parent().parent().find('.idx_row').val();
		//var a = $('.idx_radio:checked').parents('tr');
		var fullData = testCaseTable.data().toArray();
		if(count == fullData.length){
			return false;
		}else{
			var arrItem = fullData[count-1]
			fullData[count-1] = fullData[count] 
			fullData[count] = arrItem;	
		}
		testCaseTable.clear().draw()
		testCaseTable.rows.add(fullData).draw(false);	
		$('#testCaseTable_wrapper .idx_radio').eq(count).prop('checked', true);
	}
	});
	$('#row_UP').click(function(e){
	$('#saveChangeCheck').val("Y");
	
	if($("#testResultTable_wrapper").is(":visible")){
		var count = $('#testResultTable_wrapper .idx_radio:checked').parent().parent().find('.idx_row').val();
		//var a = $('.idx_radio:checked').parents('tr');
		var fullData = testResultTable.data().toArray();
		if(count - 1 == 0){
			return false;
		}else{
			var arrItem = fullData[count-1]
			fullData[count-1] = fullData[count-2] 
			fullData[count-2] = arrItem;	
		}
		testResultTable.clear().draw()
		testResultTable.rows.add(fullData).draw(false);	
		$('#testResultTable_wrapper .idx_radio').eq(count-2).prop('checked', true);
	
	}else if($("#testLogTable_wrapper").is(":visible")){
		var count = $('#testLogTable_wrapper .idx_radio:checked').parent().parent().find('.idx_row').val();
		//var a = $('.idx_radio:checked').parents('tr');
		var fullData = testLogTable.data().toArray();
		if(count - 1 == 0){
			return false;
		}else{
			var arrItem = fullData[count-1]
			fullData[count-1] = fullData[count-2] 
			fullData[count-2] = arrItem;	
		}
		testLogTable.clear().draw()
		testLogTable.rows.add(fullData).draw(false);	
		$('#testLogTable_wrapper .idx_radio').eq(count-2).prop('checked', true);
	}else if($("#testCaseTable_wrapper").is(":visible")){
		var count = $('#testCaseTable_wrapper .idx_radio:checked').parent().parent().find('.idx_row').val();
		//var a = $('.idx_radio:checked').parents('tr');
		var fullData = testCaseTable.data().toArray();
		if(count == fullData.length){
			return false;
		}else{
			var arrItem = fullData[count-1]
			fullData[count-1] = fullData[count-2] 
			fullData[count-2] = arrItem;	
		}
		testCaseTable.clear().draw()
		testCaseTable.rows.add(fullData).draw(false);	
		$('#testCaseTable_wrapper .idx_radio').eq(count).prop('checked', true);
	}
	});

```

<br>

>  ###### 수정
```
	function moveRows(direction){
	    var tableWrapper, table;
	    // 현재 보이는 테이블 확인
	    if ($("#testResultTable_wrapper").is(":visible")){
		tableWrapper = "#testResultTable_wrapper";
		table = testResultTable;
	    } else if ($("#testLogTable_wrapper").is(":visible")){
		tableWrapper = "#testLogTable_wrapper";
		table = testLogTable;
	    } else if ($("#testCaseTable_wrapper").is(":visible")){
		tableWrapper = "#testCaseTable_wrapper";
		table = testCaseTable;
	    } else {
		return;
	    }
	
	    var count = $(tableWrapper + ' .idx_radio:checked').parent().parent().find('.idx_row').val();
	    var fullData = table.data().toArray();
		    
	    if (direction == 'UP' && count - 1 == 0) return false;
	    if (direction == 'DOWN' && count == fullData.length) return false;
	
	    var swapIndex = direction == 'UP' ? count - 2 : count;
	    var arrItem = fullData[count - 1];
	    fullData[count - 1] = fullData[swapIndex];
	    fullData[swapIndex] = arrItem;
	
	    table.clear().draw();
	    table.rows.add(fullData).draw(false);
	    $(tableWrapper + ' .idx_radio').eq(swapIndex).prop('checked', true);
	}
	
	// 버튼 클릭 이벤트
	$('#row_UP').click(() => moveRows('UP'));
	$('#row_DOWN').click(() => moveRows('DOWN'));	
```

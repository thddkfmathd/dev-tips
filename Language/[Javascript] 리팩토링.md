### 1. 행 줄바꿈 함수 리팩토링 

###### 기존 source
```
$('#row_DOWN').click(function(e){
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
  }else if($("#testTestcaseTable_wrapper").is(":visible")){
    var count = $('#testTestcaseTable_wrapper .idx_radio:checked').parent().parent().find('.idx_row').val();
    //var a = $('.idx_radio:checked').parents('tr');
    var fullData = testTestcaseTable.data().toArray();
    if(count == fullData.length){
      return false;
    }else{
      var arrItem = fullData[count-1]
      fullData[count-1] = fullData[count] 
      fullData[count] = arrItem;	
    }
    testTestcaseTable.clear().draw()
    testTestcaseTable.rows.add(fullData).draw(false);	
    $('#testTestcaseTable_wrapper .idx_radio').eq(count).prop('checked', true);
  }
});
$('#row_UP').click(function(e){
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
  }else if($("#testTestcaseTable_wrapper").is(":visible")){
    var count = $('#testTestcaseTable_wrapper .idx_radio:checked').parent().parent().find('.idx_row').val();
    //var a = $('.idx_radio:checked').parents('tr');
    var fullData = testTestcaseTable.data().toArray();
    if(count == fullData.length){
									}
								});
								
							$("button.btn-deㄹ### 1. 행 줄바꿈 함수 리팩토링 ction (e) {
									var tr = $(this).closest("tr");
									testResultTable.row(tr).remove().draw(false);
								});
								
								$('select[data-name="server"]', row).change(function(e) {
									$("#serverName").val("");
									
									var server_group = $('select[data-name="server"] > option:selected', row);
									data.server_id 		= server_group.val();
									data.server_name 	= server_group.text();
									data.server_ip 		= server_group.attr("ip");
									data.server_port 	= server_group.attr("port");
								});
							
						} catch (e) {
							alert('rowCallback : ' + e.message);
						}
					},
					ajax: null
				}); //testResultTable End
				
				// 실거래 로그에서 선택한 테스트를 보여주는 테이블
				testLogTable = $('#testLogTable').DataTable({테이블행 정의}],
					rowCallback: function (row, data, index) {
						try {
							$(row).find('.idx_row').val(index + 1);
							 $(row).on("dblclick", function(e) {
								if (!e.target.className.includes('button')) {
									$("#popup-logTest-detailsLayout").modal().reset;
									$("#req_log").val(data.req_log);
									if(data.res_log==null){
										 $("#res_log").val(data.test_err_log);
									}else{
										$("#res_log").val(data.res_log);
									}
									
									var msgId = data.msg_layout_id;
									// 1) res_log에서 msgId를 추출. 2) db에서 msgId를 전달받음.
									// 위 케이스등을 통해 그릴 msg id를 찾는다. 
									drawLayout(msgId, data);
									
									$("#popup-logTest-detailsLayout").modal("show");
								} else {
									// sweetAlertErrorMessage('여기는 더블클릭이 안되는 곳이에요');
									return false;
								}
							});
							 
							
							$("button.btn-delete", row).click(function (e) {
								var tr = $(this).closest("tr");
								testLogTable.row(tr).remove().draw(false);
							});
							
							$('select[data-name="server"]', row).change(function(e) {
								$("#serverName").val("");
								
								var server_group = $('select[data-name="server"] > option:selected', row);
								data.server_id 		= server_group.val();
								data.server_name 	= server_group.text();
								data.server_ip 		= server_group.attr("ip");
								data.server_port 	= server_group.attr("port");
							});
							
						} catch (e) {
							alert('rowCallback : ' + e.message);
						}
					},
					ajax: null
				}); // testLogTable end
				testTestcaseTable = $('#testTestcaseTable').DataTable({테이블행 정의의}],
					rowCallback: function (row, data, index) {
						try {
							$(row).find('.idx_row').val(index + 1);
							 $(row).on("dblclick", function(e) {
								if (!e.target.className.includes('button')) {
									//테스트케이스 화면에서 참조하여 개발 진행
									$("#popup-testcase-detailsLayout").modal().reset;
									
									$("#popup-testcase-detailsLayout").modal("show");
								} else {
									return false;
								}
							});
							 
							
							$("button.btn-delete", row).click(function (e) {
								var tr = $(this).closest("tr");
								testTestcaseTable.row(tr).remove().draw(false);
							});
							
							$('select[data-name="server"]', row).change(function(e) {
								$("#serverName").val("");
								
								var server_group = $('select[data-name="server"] > option:selected', row);
								data.server_id 		= server_group.val();
								data.server_name 	= server_group.text();
								data.server_ip 		= server_group.attr("ip");
								data.server_port 	= server_group.attr("port");
							});
							
						} catch (e) {
							alert('rowCallback : ' + e.message);
						}
					},
					ajax:	ㄹ						$("button.btn-de 1. 행 줄바꿈 함수 리팩토링 ction (e) {al 열기
				$("button.btnRecentWork").click(function (e) {
					$('#recentWork_search_form')[0].reset();
					searchParamInsert('recentWork');
					// recentWorkTable.order(4, "desc");
					recentWorkTable.ajax.reload();
					$(".popup-recentWork").modal("show");
				});
				
				// 기존작업 선택 모달 초기화 버튼 이벤트
				$('button.btn-recentWorkReset').click(function (e) {
					$('#recentWork_search_form')[0].reset();
					searchParamInit();
					// recentWorkTable.order(4, "desc");
					recentWorkTable.ajax.reload();
				});


				$('#row_DOWN').click(function(e){
				   // 행 변경 함수 
				}); 
				$('#row_UP').click(function(e){
          // 행 변경 함수 
				});

				// 로그 리스트 Modal 호출 버튼
				$('button.btnLog').click(function (e) {
					$('#log_search_form')[0].reset();
					searchParamInsert('log');
					// logTable.order(3, "desc");
					logTable.ajax.reload();
					$("#logServerName").val("");
					$(".popup-log").modal("show");
				});
				// 로그 테이블 초기화
				$('button.btn-logReset').click(function (e) {
					$('#log_search_form')[0].reset();
					searchParamInit();
					// logTable.order(3, "desc");
					logTable.ajax.reload();
				});
				
				// 로그 리스트 Modal 호출 버튼
				$('button.btnTestcase').click(function (e) {
					$('#testcase_search_form')[0].reset();
					searchParamInsert('testcase');
					// logTable.order(3, "desc");
					testcaseTable.ajax.reload();
					$("#testcaseServerName").val("");
					$(".popup-testcase").modal("show");
				});
				// 로그 테이블 초기화
				$('button.btn-testcaseReset').click(function (e) {
					$('#testcase_search_form')[0].reset();
					searchParamInit();
					// logTable.order(3, "desc");
					testcaseTable.ajax.reload();
				});

				//선택 이벤트(결과)
				$("#resultRegression_select").click(function (e) {
					if(test_id_array.length > 0){
						$("#loading").show();
						
						$.ajax({
							type: "POST",
							url: "/api/unittest/regressionTest/result",
							data: {
								test_id_array: test_id_array
							},
							success: function(response) {
								if(response.message){
									sweetAlertSuccessMessage(response.message);
								}
								
								if (response.result) {
									var data = response.data;
									
									$("#testList-div").show();
									
									if($("#testResultTable_wrapper").is(":visible")){
									}else if($("#testTestcaseTable_wrapper").is(":visible")){
										testTestcaseTable.clear();
										$("#serverName").val("");
									}else {
										testLogTable.clear();
										$("#serverName").val("");
									}
									$("#testLogTable_wrapper").hide();
									$("#testResultTable_wrapper").show();
									$("#testTestcaseTable_wrapper").hide();

									testResultTable.rows.add(data).draw(false);
									
									$("#loading").hide();
								}
								
							},
						});
						test_id_array = [];
					}
					$(".popup-recentWork").modal("hide");
				});
				
				$("#resultRegression_cancle").click(function (e) {
					test_id_array = [];
					//$(".popup-recentWork").modal("hide");
				});
				
				//선택 이벤트(로그)
				$("#logRegression_select").click(function (e) {
					//logtest_id_array
					if(logtest_id_array.length > 0){
						$("#loading").show();
						
						$.ajax({
							type: "POST",
							url: "/api/unittest/regressionTest/log",
							data: {
								test_id_array: logtest_id_array
							},
							success: function(response) {
								if (response.message) {
									sweetAlertSuccessMessage(response.message);
								}
								
								if (respo					ajax:	ㄹㅇ						$("button.btn-de 1. 행 줄바꿈 함수 리팩토링 ction (e) {al 열기var server_id = $("#logServerName").val();
									if(server_id != ""){
										data.forEach(function(f){
											f.server_id = server_id;
										});
									}
									
									$("#testList-div").show();
									
									if($("#testLogTable_wrapper").is(":visible")){
									}else if($("#t				",) == ""){: 50, 	className: 'align-center', orderable: true },esultTable.clear().draw();#search_str').val($("#recentWork_search_str").val());ltTable.clear();

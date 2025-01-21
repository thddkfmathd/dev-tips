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
      return false;
    }else{
      var arrItem = fullData[count-1]
      fullData[count-1] = fullData[count-2] 
      fullData[count-2] = arrItem;	
    }
    testTestcaseTable.clear().draw()
    testTestcaseTable.rows.add(fullData).draw(false);	
    $('#testTestcaseTable_wrapper .idx_radio').eq(count).prop('checked', true);
  }
});
```

<br>

###### 수정
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






```
			var recentWorkTable;
			var logTable;
			var testResultTable;
			var testLogTable;
			var test_id_array = [];
			var logtest_id_array = [];
			var testcasetest_id_array = [];
			var serverList;
			var savedListTable;
			
			$(document).ready(function () {
	
				initLayout();

        // 1
				recentWorkTable = $('#recentWorkTable').DataTable({테이블행 정의}],
					rowCallback: function (row, data, index) {
						try {
							$("button.btnDetail", row).click(function (e) {
									$("#popup-logTest-detailsLayout").modal().reset;
									
									$("#req_log").val(data.req_log);
									if(data.res_log==null){
										 $("#res_log").val(data.test_err_log);
									}else{
										$("#res_log").val(data.res_log);
									}
									var msgLayout = JSON.parse(data.msg_layout_log);
									drawLayout_layoutLog(msgLayout);
									
									$("#popup-logTest-detailsLayout").modal("show");
								});
								
								$('input.result_id', row).click(function () {
									if(this.checked){
										test_id_array.push(this.value);
									}else{
										test_id_array.splice(test_id_array.indexOf(this.value), 1);
									}
									
									var checkCount = 0;
									var checkedCount = 0;
									$("input.result_id").each(function (){
										checkCount++;
										if($(this).prop("checked")){
											checkedCount++;
										}else{
										}
									});
									if(checkCount == checkedCount){
										$('input.result-check-all').prop("checked",true);
									}else{
										$('input.result-check-all').prop("checked",false);
									}
								});
								
						} catch (e) {
							alert('rowCallback : ' + e.message);
						}
					},
					drawCallback: function(settings){
							var checkCount = 0;
							var checkedCount = 0;
							$("input.result_id").each(function (){
								checkCount++;
								if($(this).prop("checked")){
									checkedCount++;
								}else{
								}
							});
							if(checkCount == checkedCount){
								$('input.result-check-all').prop("checked",true);
							}else{
								$('input.result-check-all').prop("checked",false);
							}
						},
					ajax: {
							url: '/api/MCIUnitTest_history_list/'
						}
				}); //recentWorkTable End
				
					// 행 더블클릭(결과)
					$("#recentWorkTable tbody").on("dblclick", "tr", function(){
						rowData = recentWorkTable.row(this).data();
						
						$.ajax({
							type: "POST",
							url: "/api/unittest/regressionTest/result",
							data: {
								test_id_array: [rowData.unittest_result_id]
							},
							success: function(response) {
								if (response.message) {
									sweetAlertSuccessMessage(response.message);
								}
								
								if (response.result) {
									var data = response.data;
									
									$("#testList-div").show();
									
									if($("#testResultTable_wrapper").is(":visible")){
										//testLogTable.clear();
// 										$("#testLogTable_wrapper").hide();
									}else {
										testLogTable.clear();
										$("#serverName").val("");
									}
									$("#testLogTable_wrapper").hide();
									$("#testResultTable_wrapper").show();
									testResultTable.rows.add(data).draw(false);
									
									$("#loading").hide();
								}
							}
						});
						
						
// 						$("#testList-div").show();
// 						if($("#testResultTable_wrapper").is(":visible")){
// 							$("#testLogTable_wrapper").hide();
// 						}else {
// 							$("#serverName").val("");
// 							$("#testLogTable_wrapper").hide();
// 							$("#testResultTable_wrapper").show();
// 						}
// 						testResultTable.rows.add([rowData]).draw(false);
						
					});
				
				$("input.result-check-all").click( function() {
						if(this.checked){
							$('input.result_id').each(function (){
								if($(this).prop("checked")){
								}else{
									$(this).prop("checked", true );
									test_id_array.push($(this).val());
								}
							});
						} else{
							$('input.result_id').each(function (){
								if($(this).prop("checked")){
									$(this).prop("checked", false );
									test_id_array.splice(test_id_array.indexOf($(this).val()), 1);
								}else{
								}
							});
						}
					});
				
				logTable = $('#logTable').DataTable({ 테이블행 정의}],
					rowCallback: function (row, data, index) {
						try {
							$("button.btn-detail", row).click(function (e) {
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
							});
							
							$('input.log_id', row).click(function () {
								if(this.checked){
									logtest_id_array.push(this.value);
								}else{
									logtest_id_array.splice(logtest_id_array.indexOf(this.value), 1);
								}
								
								var checkCount = 0;
								var checkedCount = 0;
								$("input.log_id").each(function (){
									checkCount++;
									if($(this).prop("checked")){
										checkedCount++;
									}else{
									}
								});
								if(checkCount == checkedCount){
									$('input.log-check-all').prop("checked",true);
								}else{
									$('input.log-check-all').prop("checked",false);
								}
							});
							
						} catch (e) {
							alert('rowCallback : ' + e.message);
						}
					},
					drawCallback: function(settings){
						var checkCount = 0;
						var checkedCount = 0;
						$("input.log_id").each(function (){
							checkCount++;
							if($(this).prop("checked")){
								checkedCount++;
							}else{
							}
						});
						if(checkCount == checkedCount){
							$('input.log-check-all').prop("checked",true);
						}else{
							$('input.log-check-all').prop("checked",false);
						}
					},
					ajax: {
							url: '/api/log_LIST'
						}
				}); // logTable end


				testcaseTable = $('#testcaseTable').DataTable({테이블행 정의의}],
					rowCallback: function (row, data, index) {
						try {
							$("button.btn-detail", row).click(function (e) {
								$("#popup-testcaseTest-detailsLayout").modal().reset;
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
								
								$("#popup-testcaseTest-detailsLayout").modal("show");
							});
							
							$('input.unittest_testcase_id', row).click(function () {
								if(this.checked){
									testcasetest_id_array.push(this.value);
								}else{
									testcasetest_id_array.splice(testcasetest_id_array.indexOf(this.value), 1);
								}
								
								var checkCount = 0;
								var checkedCount = 0;
								$("input.unittest_testcase_id").each(function (){
									checkCount++;
									if($(this).prop("checked")){
										checkedCount++;
									}else{
									}
								});
								if(checkCount == checkedCount){
									$('input.testcase-check-all').prop("checked",true);
								}else{
									$('input.testcase-check-all').prop("checked",false);
								}
							});
							
						} catch (e) {
							alert('rowCallback : ' + e.message);
						}
					},
					drawCallback: function(settings){
						var checkCount = 0;
						var checkedCount = 0;
						$("input.unittest_testcase_id").each(function (){
							checkCount++;
							if($(this).prop("checked")){
								checkedCount++;
							}else{
							}
						});
						if(checkCount == checkedCount){
							$('input.testcase-check-all').prop("checked",true);
						}else{
							$('input.testcase-check-all').prop("checked",false);
						}
					},
					ajax: {
							url: '/api/unitTest_testcase_myList'
						}
				}); // testcaseTable end
				// 행 더블클릭(로그)
				$("#logTable tbody").on("dblclick", "tr", function(){
					rowData = logTable.row(this).data();
					
					$.ajax({
						type: "POST",
						url: "/api/unittest/regressionTest/log",
						data: {
							test_id_array: [rowData.log_id]
						},
						success: function(response) {
							if (response.message) {
								sweetAlertSuccessMessage(response.message);
							}
							
							if (response.result) {
								var data = response.data;
								
								$("#testList-div").show();
								
								if($("#testLogTable_wrapper").is(":visible")){
									//testResultTable.clear();
// 									$("#testResultTable_wrapper").hide();
								}else {
									testResultTable.clear();
									$("#serverName").val("");
								}
								$("#testResultTable_wrapper").hide();
								$("#testLogTable_wrapper").show();
								testLogTable.rows.add(data).draw(false);
								
								$("#loading").hide();
							}
						}
					});
				});
				
				$("input.log-check-all").click( function() {
					//$("input.log_id").prop("checked", this.checked );
					
					if(this.checked){
						$('input.log_id').each(function (){
							if($(this).prop("checked")){
							}else{
								$(this).prop("checked", true );
								logtest_id_array.push($(this).val());
							}
						});
					} else{
						$('input.log_id').each(function (){
							if($(this).prop("checked")){
								$(this).prop("checked", false );
								logtest_id_array.splice(logtest_id_array.indexOf($(this).val()), 1);
							}else{
							}
						});
					}
				});
				$("#testcaseTable tbody").on("dblclick", "tr", function(){
					rowData = testcaseTable.row(this).data();
					
					$.ajax({
						type: "POST",
						url: "/api/unittest/regressionTest/testcase",
						data: {
							test_id_array: [rowData.testcase_id]
						},
						success: function(response) {
							if (response.message) {
								sweetAlertSuccessMessage(response.message);
							}
							
							if (response.result) {
								var data = response.data;
								
								$("#testList-div").show();
								
								if($("#testLogTable_wrapper").is(":visible")){
									//testResultTable.clear();
// 									$("#testResultTable_wrapper").hide();
								}else {
									testResultTable.clear();
									$("#serverName").val("");
								}
								$("#testResultTable_wrapper").hide();
								$("#testLogTable_wrapper").show();
								testLogTable.rows.add(data).draw(false);
								
								$("#loading").hide();
							}
						}
					});
				});
				$("input.testcase-check-all").click( function() {
					//$("input.log_id").prop("checked", this.checked );
					
					if(this.checked){
						$('input.unittest_testcase_id').each(function (){
							if($(this).prop("checked")){
							}else{
								$(this).prop("checked", true );
								testcasetest_id_array.push($(this).val());
							}
						});
					} else{
						$('input.unittest_testcase_id').each(function (){
							if($(this).prop("checked")){
								$(this).prop("checked", false );
								testcasetest_id_array.splice(testcasetest_id_array.indexOf($(this).val()), 1);
							}else{
							}
						});
					}
				});
				// 테스트 결과에서 선택한 테스트를 보여주는 테이블
				testResultTable = $('#testResultTable').DataTable({테이블 행 정의의}],
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
										var msgLayout = JSON.parse(data.msg_layout_log);
										drawLayout_layoutLog(msgLayout);
										
										$("#popup-logTest-detailsLayout").modal("show");
									} else {
										// sweetAlertErrorMessage('여기는 더블클릭이 안되는 곳이에요');
										return false;
									}
								});
								
							$("button.btn-delete", row).click(function (e) {
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
					ajax: null
				}); // testTestcaseTable end
				// 기존작업조회 modal 열기
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
								
								if (response.result) {
									var data = response.data;
									
									var server_id = $("#logServerName").val();
									if(server_id != ""){
										data.forEach(function(f){
											f.server_id = server_id;
										});
									}
									
									$("#testList-div").show();
									
									if($("#testLogTable_wrapper").is(":visible")){
									}else if($("#testTestcaseTable_wrapper").is(":visible")){
										testTestcaseTable.clear();
										$("#serverName").val("");
									}else {
										testResultTable.clear();
										$("#serverName").val("");
									}
									$("#testResultTable_wrapper").hide();
									$("#testLogTable_wrapper").show();
									$("#testTestcaseTable_wrapper").hide();
									testLogTable.rows.add(data).draw(false);
									
									$("#loading").hide();
								}
							}
						});
						logtest_id_array = [];
					}
					$(".popup-log").modal("hide");
				});
				
				$("#logRegression_cancle").click(function (e) {
					logtest_id_array = [];
					//$(".popup-log").modal("hide");
				});
				//선택 이벤트(로그)
				$("#testcaseRegression_select").click(function (e) {
					//logtest_id_array
					if(testcasetest_id_array.length > 0){
						$("#loading").show();
						
						$.ajax({
							type: "POST",
							url: "/api/unittest/regressionTest/testcase",
							data: {
								test_id_array: testcasetest_id_array
							},
							success: function(response) {
								if (response.message) {
									sweetAlertSuccessMessage(response.message);
								}
								
								if (response.result) {
									var data = response.data;
									
									var server_id = $("#testcaseServerName").val();
									if(server_id != ""){
										data.forEach(function(f){
											f.server_id = server_id;
										});
									}
									
									$("#testList-div").show();
									
									if($("#testLogTable_wrapper").is(":visible")){
										testLogTable.clear();
										$("#serverName").val("");
									}else if($("#testTestcaseTable_wrapper").is(":visible")){
									}else {
										testResultTable.clear();
										$("#serverName").val("");
									}
									$("#testResultTable_wrapper").hide();
									$("#testLogTable_wrapper").hide();
									$("#testTestcaseTable_wrapper").show();
									testTestcaseTable.rows.add(data).draw(false);
									
									$("#loading").hide();
								}
							}
						});
						testcasetest_id_array = [];
					}
					$(".popup-testcase").modal("hide");
				});
				
				$("#testcaseRegression_cancle").click(function (e) {
					logtest_id_array = [];
					//$(".popup-log").modal("hide");
				});

				//실행
				$("#testExecute").click(function (e) {
					/* 서버 선택을 row에서 각각 선택하게 변경 
					var server_group = $("#serverName > option:selected");
					if(server_group.val() == ""){
						console.log("서버선택 x");
						sweetAlertWarningMessage("전송 서버를 선택해 주세요.");
						return false;
					}
					*/
					if(testResultTable.data().toArray().length < 0){
						sweetAlertWarningMessage("테스트할 결과 혹은 로그를 선택해 주세요.");
						return false;
					}
					
					var flag = true;
					$("select.serverList").each(function (){
						if($(this).val() == ""){
							sweetAlertWarningMessage("전송 서버를 선택해 주세요.");
							$(this).focus();
							flag = false;
							return false;
						}
					});	
					if(!flag){
						return false;
					}
							
					testStart();
					
					var param = {
						//server_id 			: server_group.val(),
						//server_name 		: server_group.text(),
						//server_ip 			: server_group.attr("ip"),
						//server_port 		: server_group.attr("port"),
						test_data			: []
					}
					var source;
					var test_data_list = [];
					$(testResultTable.data().toArray()).each(function(idx, test_map){
						test_data_list[idx] = test_map.unittest_result_id;
					});
					$(testLogTable.data().toArray()).each(function(idx, test_map){
						test_data_list[idx] = test_map.log_id;
					});
					$(testTestcaseTable.data().toArray()).each(function(idx, test_map){
						test_data_list[idx] = test_map.log_id;
					});
					if($("#testResultTable_wrapper").is(":visible")){
						//param.test_data = testResultTable.data().toArray();
						//param.test_data = test_data_list;
						source = "result";
					}else if($("#testLogTable_wrapper").is(":visible")){
						//param.test_data = testLogTable.data().toArray();
						//param.test_data = test_data_list;
						source = "log";
					}else if($("#testTestcaseTable_wrapper").is(":visible")){
						//param.test_data = testLogTable.data().toArray();
						//param.test_data = test_data_list;
						source = "testcase";
					}
					//console.log(param);
					
					$.ajax({
						type: "post",
						url: "/api/regressionTest_execute/"+source,
						data: {
							test_id_array : test_data_list
							, project_id : $.cookie('project_id')
							
						},
						success: function(response){
							if(response.result){
								window.location.href = '${pageContext.request.contextPath}/unitTest/results.view';
							}else{
								tesetEnd();
								alert(response.message);
							}
						}
					});
					window.location.href = '${pageContext.request.contextPath}/unitTest/results.view';
				});
				
				//서버선택
				$("#serverName").change(function (e){
					var thisVal = $("#serverName").val();
					$("select.serverList").val($("#serverName").val()).trigger('change');
					$("#serverName").val(thisVal);
				});
				
				// 불러오기 버튼 선택시 보여주는 테이블
				savedListTable = $('#savedListTable').DataTable({
					order: [[5, "desc"]], // 등록일
					columns: [
						{
							title: '', data: 'null', defaultContent: '', className: 'align-center', width: 30,
							render: function (value, idx, data) {
								var render_str = '';
								render_str += '<button type="button" class="btn btn-icon btn-sm btn-xsmall btn-primary btn-select"><i class="fas fa-check"></i></button>';
								return render_str;
							}
						},
						{ title: "저장명", 	data: 'saved_name', 	name: 'saved_name', className: 'align-left', orderable: true, searchable: true },
						{ 
							title: "테스트구분", data: 'data_source', 	name: 'data_source', width: 120,	className: 'align-center', orderable: true ,
							render: function (value, idx, data) {
								var render_str = "";
								if(data.data_source == 'result'){
									render_str += '테스트 결과';
								} else if(data.data_source == 'log'){
									render_str += '실 거래로그 결과';
								} else if(data.data_source == 'testcase'){
									render_str += '테스트케이스';
								}
								return render_str;
							}
						},
						{ title: "목록 수", 	data: 'id_array_count', name: 'id_array_count', width: 50, 	className: 'align-center', orderable: true },
						{ title: "공유여부", data: "share_yn", name: 'share_yn', className: 'align-center n-dblclick', width: 70 },
						{ title: "등록일", 	data: 'inserted_dt', 	name: 'inserted_dt', 	width: 100, className: 'align-center type-datetime', orderable: true },
						{
							title: '',
							data: 'null', defaultContent: '', className: 'align-center type-button', width: 50,
							render: function (value, idx, data) {
								var render_str = '';
								render_str += '<button type="button" class="btn btn-outline-danger btn-round-custom btn-delete">삭제</button>';
								return render_str;
							}
						}
					],
					rowCallback: function (row, data, index) {
						try {
							$("button.btn-select", row).click(function () {
								loadList(data.data_source, data.id_array);
							});
							
							$("button.btn-detail", row).click(function (e) {
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
							});
							
							$("button.btn-delete", row).click(function () {
								Swal.fire({
									title: "[" + data.saved_name +  "]",
									html: '선택된 테스트를 삭제하시겠습니까?' ,
									icon: 'question',
									iconColor: '#f27474',
									showCancelButton: true,
									//confirmButtonColor: '#3085d6',
									//cancelButtonColor: '#d33',
									confirmButtonText: '확인',
									cancelButtonText: '취소'
								}).then(function(result) {
									if (result.value) {
										//"확인" 버튼을 눌렀을 때 작업할 내용을 이곳에 넣어주면 된다. 
										$.ajax({
											type: "POST",
											url: '/api/regressionTest_delete',
											data: {
												saved_id : data.saved_id
											},
											success: function (response) {
												if (response.result) {
													var tr = $(this).closest("tr");
													savedListTable.row(tr).remove().draw(false);
													window.location.reload();
												} else {
													alert(response.message);
												}
											}
										});
									} else {
										table.ajax.reload();
										return false;
									}
								});
							});
							var share_str='<label class="switch"><input id="toggleBtn" type="checkbox" data-toggle="toggle"><span class="slider round n-dblclick"></span></label>';
							$('td:eq(4)', row).html(share_str);
							if(data.share_yn == 'Y'){
								$("#toggleBtn",row).prop('checked', true);
								}else{
									$("#toggleBtn",row).prop('checked', false);
								}
							
							$("#toggleBtn", row).change(function () {
								var param = {
									scenario_id: data.scenario_id,
									share_yn : ($(this).prop('checked') ? 'Y' :'N')
								};
								$.ajax({
									type: "POST",
									url: '/api/regressionTest_share',
									data: {
										saved_id : data.saved_id
									},
									success: function (response) {
										if (response.result) {
										} else {
											alert(response.message);
										}
									}
								});
								
							});
						} catch (e) {
							alert('rowCallback : ' + e.message);
						}
					},
					ajax: {
						url: '/api/regressionTest_savedList'
					}
				}); // savedListTable end
				
				$("#listReset").click(function (e){
					testResultTable.clear().draw();
					testLogTable.clear().draw();
					$("#serverName").val("");
				});
				
				$("#saveList").click(function (e){
					var timezoneOffset = new Date().getTimezoneOffset() * 60000;
					var today = new Date(Date.now() - timezoneOffset).toISOString().slice(0,10);
					$("#saveName").val(today+"_저장된 회귀테스트");//.focus(function(){ $(this).select(); });
					$("#saveName").focus(function(){$(this).select();});
					$(".popup-saveName").modal("show");
				});
				$("#saveBtn").click(function (e){
					if($("#saveName").val().trim() == ""){
						sweetAlertErrorMessage("저장명을 입력해주세요.");
						$("#saveName").focus();
						return false;
					}
					
					Swal.fire({
						title: "[" + $("#saveName").val() +  "]",
						text: '회귀 테스트를 저장하시겠습니까?' ,
						icon: 'question',
						showCancelButton: true,
						//confirmButtonColor: '#3085d6',
						//cancelButtonColor: '#d33',
						confirmButtonText: '확인',
						cancelButtonText: '취소'
					}).then(function(result) {
						if (result.value) {
							//"확인" 버튼을 눌렀을 때 작업할 내용을 이곳에 넣어주면 된다. 
							var array; 
							var source;
							if($("#testResultTable_wrapper").is(":visible")){
								array = testResultTable.data().toArray();
								source = "result";
							}else if($("#testLogTable_wrapper").is(":visible")){
								array = testLogTable.data().toArray();
								source = "log";
							}else if($("#testTestcaseTable_wrapper").is(":visible")){
								array = testLogTable.data().toArray();
								source = "testcase";
							}
							var idArray=[];
							$.each(array, function (idx, data) {
								if(source == "result"){
									idArray.push(data.unittest_result_id);
								}else{
									idArray.push(data.log_id);
								}
							});
							
							//console.log("save the ",source, " list [] : ",idArray);
							
							$.ajax({
								type: "POST",
								url: "/api/regressionTest_save",
								data: {
									saveName 	: $("#saveName").val(),
									source 		: source,
									array 		: idArray
								},
								success: function(response){
									if(response.result){
										$(".popup-saveName").modal("hide");
									}else{
										sweetAlertErrorMessage(response.message);
									}
								}
							});
						} else {
							table.ajax.reload();
							return false;
						}
					});
				});
				$("#loadList").click(function (e){
					// savedListTable.order(3, "desc");
					savedListTable.ajax.reload();
					$(".popup-savedList").modal("show");
			 	});
			}); // $(document).ready end
			
			//이중모달 사용시 스크롤 문제 해결
			$(document).on('hidden.bs.modal', function (event) {
				if ($('.modal:visible').length) {
					$('body').addClass('modal-open');
				}
			});
			
			// 테이블 검색 변수로 사용되는 태그들 초기화
			function searchParamInit() {
				$('#search_gubun').attr('data-name', '');
				$('#search_gubun').val('');
				$('#search_service').val('');
				$('#search_str').val('');
				$('#search_project').val('');
				$('#search_start').attr("data-name", '');
				$('#search_start').val('');
				$('#search_end').attr("data-name", '');
				$('#search_end').val('');
			}
			
			// 테이블 검색 변수로 사용되는 태그에 검색 데이터 삽입
			function searchParamInsert(category) {
				switch(category) {
				case "recentWork":
					$('#search_gubun').attr('data-name', $("#recentWork_search_gubun").attr("data-name"));
					$('#search_gubun').val($("#recentWork_search_gubun").val());
					$('#search_service').val($("#recentWork_search_service").val());
					$('#search_str').val($("#recentWork_search_str").val());
					$('#search_project').val($("#recentWork_search_project").val());
					$('#search_str_groupCode').val($("#recentWork_group_cd").val());
					$('#search_start').attr("data-name", $("#recentWork_search_start").attr("data-name"));
					$('#search_start').val($("#recentWork_search_start").val());
					$('#search_end').attr("data-name", $("#recentWork_search_end").attr("data-name"));
					$('#search_end').val($("#recentWork_search_end").val());
					break;
				case "log":
					$('#search_gubun').attr('data-name', $("#log_search_gubun").attr("data-name"));
					$('#search_gubun').val($("#log_search_gubun").val());
					$('#search_service').val($("#log_search_service").val());
					$('#search_str').val($("#log_search_str").val());
					$('#search_project').val($("#log_search_project").val());
					$('#search_str_groupCode').val($("#log_group_cd").val());
					$('#search_start').attr("data-name", $("#log_search_start").attr("data-name"));
					$('#search_start').val($("#log_search_start").val());
					$('#search_end').attr("data-name", $("#log_search_end").attr("data-name"));
					$('#search_end').val($("#log_search_end").val());
					break;
				case "testcase":
					$('#search_gubun').attr('data-name', $("#testcase_search_gubun").attr("data-name"));
					$('#search_gubun').val($("#testcase_search_gubun").val());
					$('#search_service').val($("#testcase_search_service").val());
					$('#search_str').val($("#testcase_search_str").val());
					$('#search_project').val($("#testcase_search_project").val());
					$('#search_str_groupCode').val($("#testcase_group_cd").val());
					$('#search_start').attr("data-name", $("#testcase_search_start").attr("data-name"));
					$('#search_start').val($("#testcase_search_start").val());
					$('#search_end').attr("data-name", $("#testcase_search_end").attr("data-name"));
					$('#search_end').val($("#testcase_search_end").val());
					break;
				default :
					break;
				}
			};
			
			function validateSearchBoxForm(category) {
				event.preventDefault();
				var category = category;
				// 검색 버튼 입력 이벤트 searchParam에 검색조건 값 저장.
				searchParamInsert(category);
				if (category == "recentWork") {
					recentWorkTable.ajax.reload();
				} else if (category == 'log'){
					logTable.ajax.reload();
				} else if (category == 'testcase'){
					testcaseTable.ajax.reload();
				} 
				return false;
			};
			
			function testStart(){
				$("#loading").show();
				$("#testExecute").prop("disabled", true);
			}
			function tesetEnd(){
				$("#loading").hide();
				$("#testExecute").prop("disabled", false);
			}
			
			//불러오기, log+result 동시에 초기화
			function loadList(source, array){
				var idArray = array.split(",");
				
				$(".popup-savedList").modal("hide");
				$("#loading").show();
				
				$.ajax({
					type: "POST",
					url: "/api/regressionTest_load",
					data: {
						source 	: source,
						array 	: idArray
					},
					success: function(response){
						if(response.result){
							var data = response.data;
							
							$("#testList-div").show();
							
							if(source == "log"){
								testLogTable.clear();
								testResultTable.clear();
								$("#serverName").val("");
								$("#testResultTable_wrapper").hide();
								$("#testTestcaseTable_wrapper").hide();
								$("#testLogTable_wrapper").show();
								testLogTable.rows.add(data).draw(false);
							}if(source == "testcase"){
								testLogTable.clear();
								testResultTable.clear();
								$("#serverName").val("");
								$("#testResultTable_wrapper").hide();
								$("#testTestcaseTable_wrapper").show();
								$("#testLogTable_wrapper").hide();
								testcaseTable.rows.add(data).draw(false);
							}else {
								testLogTable.clear();
								testResultTable.clear();
								$("#serverName").val("");
								$("#testLogTable_wrapper").hide();
								$("#testTestcaseTable_wrapper").hide();
								$("#testResultTable_wrapper").show();
								testResultTable.rows.add(data).draw(false);
							}
							$("#loading").hide();
						}else{
							$("#loading").hide();
							alert(response.message);
						}
					}
				});
			}
```

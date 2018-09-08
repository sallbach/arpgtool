# ARSSQLR - SQL-Tools

xxxxx

	dcl-s pInResultSet int(10) const;
	dcl-s pAnA varchar(20);
	dcl-s pBoIndA ind;
	dcl-s pNuB packed(10, 5);
	dcl-s pBoIndB ind;
	dcl-s pBoErrB ind;
	
	inResultSet = SqlStatement('Select a, b from table1');
	dow SqlRsNext(inResultSet);
	  
		pAnA = SqlRsGetString(inResultSet:1:pBoIndA);
		pNuB = SqlRsGetDecimal(inResultSet:2); 
		pBoIndB = SqlRsSetWasNull(inResultSet); // pNuB is NULL
		pBoErrB = SqlRsSetHasErrors(inResultSet); // data mapping error ?
	  
	enddo;
	SqlRsClose(pinResultSet);
	
xxxxx

	dcl-s pInPreparedSt int(10) const;
	dcl-s pInResultSet int(10) const;
	dcl-s pInAIdx int(10);
	dcl-s pAnA varchar(20);
	dcl-s pInBIdx int(10);
	dcl-s pNuB packed(10, 5);
	
	pInPreparedSt = SqlPreparedStatement('Select b, a from xyz');
	
	pInAIdx = SqlRsFindColumn(pInPreparedSt,'a');
	pInBIdx = SqlRsFindColumn(pInPreparedSt,'b');
	
	inResultSet = SqlPreparedStatementExecuteQuery(pInPreparedSt);
	dow SqlRsNext(pInResultSet);
		pAnA = SqlRsGetString(inResultSet:pInAIdx);
	enddo;
	SqlRsClose(pInResultSet);

    // execute again
	inResultSet = SqlPreparedStatementExecuteQuery(pInPreparedSt);
	dow SqlRsNext(pInResultSet);
		pNuB = SqlRsGetDecimal(inResultSet:pInBIdx); 
	enddo;
	SqlRsClose(pInResultSet);    	
	
	// close and free all resources 
	SqlPreparedStatementClose(pInPreparedSt);
	
Please check the unit test module ARPG_TEST/T_ARSSQLR too.

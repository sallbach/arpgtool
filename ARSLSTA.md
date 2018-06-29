# ARSLSTA - LogStash functions

The logstash functions help you to transfer the complete or parts of a joblog via logstash to elastic search. It's also possible to use any other logging framework with more or less effort. Maybe your only have to implement a new connector or an new formatter if json(-format) is not the right choice. 


## The job which should be logged :
To register a job for monitoring, you have to change your program, so it calls one of the following two options at the beginning of the job :

1.  ILE function LogStashStartTask() or
2.  Command STRLOGSTA

Both transfer the complete job log via the API "Control Job Log Output (QMHCTLJL)" to the given (standard) file. You should consider to change your job description (CHGJOBD LOG(level) JOBMSGQMX(2) JOBMSGQFL(*WRAP)) to get the log information faster (JOBMSGQMX(2)) and to select the rightextmbr infds log level (LOG(level)). 

If you only want to trace the whole job you are finished now and can continue with the next step.
 
If your job handles different tasks which should be logged separately, you need start/end messages before/after your tasks. If your program already generates such messages (min. start messages), then your program is ready for logging, you only have to implement an own version of the LogStashXXXXX(). Otherwise you have to change your program and add start messages or use the functions LogStashStartTask()/LogStashEndTask() or the commands STRLOGTASK/ENDLOGTASK. 

## Start the logger :
Now you can start the logger job to monitor the file in which all your jobs write there job log.
The logger can be started with the command STRLOGGER.  


## Example :


### Program to log :


    dcl-proc YourPgm;
    
      LogStashStartLogger(); // change joblog QMHCTLJL 
      YourTask(); // run task
      
    end-proc 
    //-------------------------------------------------
    dcl-proc YourTask;
    
      LogStashStartTask(); // send start message QMHSNDPM
      LogStashSetVar('uuid':varUuid); // set additional variable uuid for all messages of this task
      LogStashSetVar('info':'special info'); // one more variable 
      
      doSomethingToLog()
    
      LogStashEndTask(); // send end message QMHSNDPM
    end-proc 


### Logger :

Submit the logger :

    SBMJOB CMD(STRLOGGER URI('socket://logstash-server:1234'));
    
Sends messages in the following format :

    {"@timestamp":"2017-08-01T17:17:58.141+02:00",
     "@version":1,
     "id":"CPF9897",
     "message":"first level message",
     "seclvl":"second level message",
     "level":"10",
     "type":"DIAG",   
     "hostname":"systemname",
     "job":"123456/userid/jobname",
     "thread":"8D14",
     "user":"userid",
     "sender_pgm":"pgm_name",
     "sender_module":"module_name",
     "sender_procedure":"procedure_name",
     "sender_stmt":"1234",
     "receiver_pgm":"pgm_name",
     "receiver_module":"module_name",
     "receiver_procedure":"procedure_name",
     "receiver_stmt":"1234",
     "uuid":"067e6162-3b6f-4ae2-a171-2470b63dff00",  
     "info":"special info"  
    }
    

## Interface
If you have special needs concerning the message reader or the message formatter you can copy the needed module source, re-implement the affected functions and build your own service program, which then can be used by the logger. 
The following functions can be re-implemented by you :

*   first_item
*	ddd
*   second_item
 
ddd


Please check the unit test module ARPG_TEST/T_ARSLSTA too.

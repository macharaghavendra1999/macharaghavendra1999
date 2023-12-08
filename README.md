global class batchclssc implements Database.Batchable<sObject> {
    global Database.QueryLocator start(Database.BatchableContext bc){
        string query='select id ,Name,AnnualRevenue,AccountNumber from Account';
        return Database.getQueryLocator(query);
    }
    global void Execute(Database.BatchableContext bc,list<sObject> para){
     
        if(!para.isEmpty()){
           // for(sObject sobj:para){
         list<Account> acclst=(list<Account>)para;//new list<Account>();
                     for(integer count=1;count<=3;count++){
               Account acc=new Account();//(Account)sobj;
            
                   if(count!=2){
                    acc.Name='apexbatch'+count;
                   }
                acc.AnnualRevenue=90000;
                acc.AccountNumber='5561233';
                acclst.add(acc);
                     
               }
         //  }
            if(!acclst.isEmpty()){
             Database.SaveResult[] result=Database.insert(acclst,false);
                if(!acclst.isEmpty()){
                    for(Database.SaveResult res:result){
                        if(res.isSuccess()){
                            system.debug('Get Id'+res.getId());
                        }
                        else{
                            for(Database.Error err:res.getErrors()){
                                system.debug('Get error:'+err.getStatusCode()+''+'Get Fields'+err.getFields());
                            }
                        }
                    }
                }
            }     
        }
        
    }
    global void Finish(Database.BatchableContext bc){
        
         AsyncApexJob   async=[SELECT Id, Status, JobItemsProcessed, TotalJobItems, 
                              NumberOfErrors, LastProcessed FROM AsyncApexJob
                             where id=:bc.getJobId()];
        system.debug('job details'+async);
    }

}

<!---
macharaghavendra1999/macharaghavendra1999 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

# utl-intraclass-correlation-coefficients-continuous-scales
Intraclass correlation coefficients continuous scales 
    Intraclass correlation coefficients continuous scales                                                                                              
                                                                                                                                                       
    I am very rusty on this topic.                                                                                                                     
                                                                                                                                                       
    R has a packages for Inter and Inra Rater Analysis                                                                                                 
                                                                                                                                                       
    see                                                                                                                                                
    https://cran.r-project.org/web/packages/irr/irr.pdf                                                                                                
                                                                                                                                                       
    SOAPBOX ON                                                                                                                                         
      It appears to me that Python is bit behind R for niche Statistical Analysis.                                                                     
      Perhaps the gap is closing, but not yet.                                                                                                         
    SOAPBOX OFF                                                                                                                                        
                                                                                                                                                       
    github                                                                                                                                             
    https://tinyurl.com/y3pgwl69                                                                                                                       
    https://github.com/rogerjdeangelis/utl-intraclass-correlation-coefficients-continuous-scales                                                       
                                                                                                                                                       
    Another Reference                                                                                                                                  
    https://tinyurl.com/yyq2ggvk                                                                                                                       
    https://www.datanovia.com/en/lessons/inter-rater-reliability-analyses-quick-r-codes/#intraclass-correlation-coefficients-continuous-scales         
                                                                                                                                                       
    SAS Forum                                                                                                                                          
    https://communities.sas.com/t5/SAS-Programming/Intra-rater-reliability/m-p/677472                                                                  
                                                                                                                                                       
                                                                                                                                                       
    /*                   _                                                                                                                             
    (_)_ __  _ __  _   _| |_                                                                                                                           
    | | `_ \| `_ \| | | | __|                                                                                                                          
    | | | | | |_) | |_| | |_                                                                                                                           
    |_|_| |_| .__/ \__,_|\__|                                                                                                                          
            |_|                                                                                                                                        
    */                                                                                                                                                 
    options validvarname=upcase;                                                                                                                       
                                                                                                                                                       
    libname sd1 "d:/sd1";                                                                                                                              
                                                                                                                                                       
    data sd1.have;                                                                                                                                     
    input id judge1 judge2 @@;                                                                                                                         
    cards4;                                                                                                                                            
    1 1.05 1.02 2 1.08 1.08 3 1.08 1.11 4 1.05 1.08 5 0.93 0.99 6 1.02 1.05                                                                            
    7 0.96 0.96 8 1.08 1.05 9 0.90 0.96 10 1.05 1.08 11 1.05 1.08 12 0.99                                                                              
    0.90 13 0.93 0.99 14 0.99 1.08 15 0.96 1.02 16 1.02 1.05 17 1.00 0.93 18                                                                           
    0.99 1.05 19 0.96 0.99 20 1.02 1.02 21 0.96 1.08 22 1.05 1.05 23 1.02                                                                              
    1.05 24 0.9 0.93 25 1.08 1.08 26 0.99 0.90 27 0.96 1.02 28 1.05 1.02                                                                               
    ;;;;                                                                                                                                               
    run;quit;                                                                                                                                          
                                                                                                                                                       
                                                                                                                                                       
    SD1.HAVE total obs=28                                                                                                                              
                                                                                                                                                       
     ID    JUDGE1    JUDGE2                                                                                                                            
                                                                                                                                                       
      1     1.05      1.02                                                                                                                             
      2     1.08      1.08                                                                                                                             
      3     1.08      1.11                                                                                                                             
      4     1.05      1.08                                                                                                                             
      5     0.93      0.99                                                                                                                             
    ...                                                                                                                                                
                                                                                                                                                       
    /*           _               _                                                                                                                     
      ___  _   _| |_ _ __  _   _| |_                                                                                                                   
     / _ \| | | | __| `_ \| | | | __|                                                                                                                  
    | (_) | |_| | |_| |_) | |_| | |_                                                                                                                   
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                                                  
                    |_|                                                                                                                                
    */                                                                                                                                                 
                                                                                                                                                       
     Single Score Intraclass Correlation                                                                                                               
                                                                                                                                                       
       Model: twoway                                                                                                                                   
       Type : agreement                                                                                                                                
                                                                                                                                                       
       Subjects = 28                                                                                                                                   
         Raters = 2                                                                                                                                    
       ICC(A,1) = 0.592                                                                                                                                
                                                                                                                                                       
     F-Test, H0: r0 = 0 ; H1: r0 > 0                                                                                                                   
     F(27,25.8) = 4.17 , p = 0.000253                                                                                                                  
                                                                                                                                                       
     95%-Confidence Interval for ICC Population Values:                                                                                                
      0.293 < ICC < 0.787                                                                                                                              
                                                                                                                                                       
    /*                                                                                                                                                 
     _ __  _ __ ___   ___ ___  ___ ___                                                                                                                 
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|                                                                                                                
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                                                                
    | .__/|_|  \___/ \___\___||___/___/                                                                                                                
    |_|                                                                                                                                                
    */                                                                                                                                                 
                                                                                                                                                       
    %utl_submit_r64('                                                                                                                                  
    library(irr);                                                                                                                                      
    library(haven);                                                                                                                                    
    have<-as.matrix(read_sas("d:/sd1/have.sas7bdat")[,-1]);                                                                                            
    have;                                                                                                                                              
    sink("d:/txt/model_summary.txt");                                                                                                                  
    icc(                                                                                                                                               
      have, model = "twoway",                                                                                                                          
      type = "agreement", unit = "single"                                                                                                              
      );                                                                                                                                               
    sink();                                                                                                                                            
    ');                                                                                                                                                
                                                                                                                                                       
    data _null_;                                                                                                                                       
     infile "d:/txt/model_summary.txt";                                                                                                                
     input;                                                                                                                                            
     put _infile_;                                                                                                                                     
    run;quit;                                                                                                                                          
                                                                                                                                                       

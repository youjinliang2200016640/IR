---
runme:
  id: 01HX3JDCMYPJTG8V0ZZVVK6PY8
  version: v3
---

# codeEval

## File Structure

### data.json

该文件以JSON格式存储的从164项HumanEval大模型代码生成测试任务中抽取的28项代码测试任务，抽取方式为`index(task_id) % 6 == 0`


### data.jsonl

该文件以JSONL格式存储的`data.json`中的数据

### data_with_output.json

该文件在`data.json`数据的基础上添加了每一个任务的标准输出

```python {"id":"01HX3M39M86E4SHHMW7Y6Z8SFY"}

# data format of codeEval/data_with_output.json
# data.json have the same format as data_with_output.json but without latter 4 items
"""            
    {"{{task_id}}":{
        "task_id":str,                  
        "prompt":str[code],
        "entry_point":str[function_name],
        "canonical_solution":str[code],
        "test":str[code],
        "contract":str[assert_code]
        "base_input":List[input],
        "plus_input":List[input],
        
        "base":List[output],
        "base_time":List[float],
        "plus":List[output],
        "plus_time":List[float],
        }
    }
"""
data_format_codeEval_ins =   {
    "HumanEval/0":{
        "task_id":str,                  
        # task_id;id;the unique identifier for a evaluation task    
        "prompt":str,                   
        # prompt;def function():;the function definition statement and description doc  ，utilize "{prompt}+{canonical_solution}" to generate the executable standard solution function          
        "entry_point":str,              
        # entry_point;function_name;the function name                                               
        "canonical_solution":str,       
        # canonical_solution;standard_function_body;the standard solution function body,utilize "{prompt}+{canonical_solution}" to generate the executable standard solution function                           
        "base_input":List[Any],         
        # base_input;List[base_test_input];a list of base test inputs                                      
        "plus_input":List[Any],         
        # plus_input;List[plus_test_input];a list of plus test inputs                                      
        "base":List[Any],               
        # base;List[base_test_answer];a list of standard answers of the base test inputs              
        "base_time":List[float],        
        # base_time;List[base_test_runtime];a list of the time spent in microseconds on each base test input
        "plus":List[Any],               
        # plus;List[plus_test_answer];a list of standard answers of the plus test inputs           
        "plus_time":List[float],        
        # plus_time;List[plus_test_runtime];a list of the time spent in microseconds on each base test input
    
    }
}  

# data format of codeEval/code_execute/*.json
"""
    {
        "{{task_id}}":{
            "info":{
                "task_id":"{{task_id}}",
                "prompt":"{{prompt}}",
                "entry_point":"{{entry_point}}",
                "canonical_solution":"{{canonical_solution}}",
                "base_input":"{{base_input}}",
                "plus_input":"{{plus_input}}",
                "atol":float,
            },
            "expected_output":{
                "base":"{{base}}",
                "base_time":"{{base_time}}",
                "plus":"{{plus}}",
                "plus_time":"{{plus_time}}",
            },
            "code_LLM":"{{solution}}",
            "actual_output":{
                "base":"{{base}}",
                "base_time":"{{base_time}}",
                "plus":"{{plus}}",
                "plus_time":"{{plus_time}}",
            }
        }
    }
""" 
data_format_execute_model_code = {
        "HumanEval/0":{                         
            # task_id;id;the unique identifier for a evaluation task   
            "info":{                            
                # info;basic info of task;
                "task_id":"HumanEval/0",        
                # task_id;id;the unique identifier for a evaluation task   
                "prompt":str,                   
                # prompt;def function():;the function definition statement and description doc ,utilize "prompt+canonical_solution" to generate the executable standard solution function 
                "entry_point":str,              
                # entry_point;function_name;the function name                                    
                "canonical_solution":str,       
                # canonical_solution;standard_function_body;the standard solution function body,utilize "prompt+canonical_solution" to generate the executable standard solution function      
                "base_input":List[Any],         
                # base_input;List[base_test_input];a list of base test inputs    
                "plus_input":List[Any],         
                # plus_input;List[plus_test_input];a list of plus test inputs   
                "atol":Union[int,float],        
                # atol;percision;the tolerance bias to judge the equivalence of two float values 
            },
            "expected_output":{
                "base":List[Any],               
                # base;List[base_test_answer];a list of standard answers of the base test inputs       
                "base_time":List[float],        
                # base_time;List[base_test_runtime];a list of the time spent in microseconds on each base test input
                "plus":List[Any],               
                # plus;List[plus_test_answer];a list of standard answers of the plus test inputs         
                "plus_time":List[float],        
                # plus_time;List[plus_test_runtime];a list of the time spent in microseconds on each base test input
            },
            "code_LLM":str,                     
            # code_LLM;code LLM generated after sanitizing
            "actual_output":{
                "base":List[Any],               
                # base;List[base_test_answer];a list of running outputs of code LLM generated of the base test inputs 
                "base_time":List[float],        
                # base_time;List[base_test_runtime];a list of the time spent in microseconds on each base test input 
                "plus":List[Any],               
                # plus;List[plus_test_answer];a list of running outputs of code LLM generated of the plus test inputs  
                "plus_time":List[float],        
                # plus_time;List[plus_test_runtime];a list of the time spent in microseconds on each base test input
            }
        }
    }  
"""
    {
        "percision":{
            "base":float,
            "plus":float,
            "mean":float
        },
        "AC":int,
        "PASS":float,
        "syntax_error_ratio":{
            "base":float,
            "plus":float,
            "mean":float
        },
        "accuracy_list":{
            "base_accuracy_list":List[float],
            "plus_accuracy_list":List[float]
        },
        "syntax_error_list":{
            "base_syntax_error_list":List[float],
            "plus_syntax_error_list":List[float]
        },
        "notImplemented_ratio_list":List[float],
        "{{task_id}}":{
            "base_eval":List[bool],
            "base_num":int,
            "base_AC":int,
            "base_TLE":int,
            "base_accuracy":float,
            "base_exp_time":List[time],
            "base_actual_time":List[time],
            
            "plus_eval":List[bool],
            "plus_num":int,
            "plus_AC":int,
            "plus_TLE":int,
            "plus_accuracy":float,
            "plus_exp_time":List[time],
            "plus_actual_time":List[bool],
            
            "accuracy":float
        }
    }
"""
data_format_evaluate_solution =  {
        "percision":{                                   
        # percision;the test case pass ratio                       
            "base":float,                               
            # base;the average base test case pass ratio of all tasks
            "plus":float,                               
            # plus;the average plus test case pass ratio of all tasks
            "mean":float,                               
            # mean;the average all_test case pass ratio of all tasks
        },
        "AC":int,                                       
        # AC;the number of test cases that the code LLM generated passed
        "PASS":float,                                   
        # PASS;the ratio that the code LLM generated pass all test cases
        "syntax_error_ratio":{                          
        # syntax_error_ratio;the ratio that the code LLM generated occures syntax error when running  
            "base":float,                               
            # base;the ratio in average that the code LLM generated occures syntax error when running the base test cases
            "plus":float,                               
            # plus;the ratio in average that the code LLM generated occures syntax error when running the plus test cases
            "mean":float                                
            # mean;the ratio in average that the code LLM generated occures syntax error when running all test cases
        },
        "accuracy_list":{                               
        # accuracy;a list of  test case pass ratio for each task                            
            "base_accuracy_list":List[float],           
            # base_accuracy_list;a list of base test case pass ratio for each task    
            "plus_accuracy_list":List[float]            
            # plus_accuracy_list;a list of plus test case pass ratio for each task    
        },
        "syntax_error_list":{                           
        # syntax_error_list;a list of syntax error ratio for each task
            "base_syntax_error_list":List[float],       
            # base_syntax_error_list;a list of syntax error ratio for each task in the base test cases
            "plus_syntax_error_list":List[float]        
            # plus_syntax_error_list;a list of syntax error ratio for each task in the plus test cases
        },
        "notImplemented_ratio_list":List[float],        
        # notImplemented_ratio_list; a list of ratio that code LLM generated return notImplemented 代码运行返回notImplemented的比率
        "HumanEval/0":{                                 
        # task_id;evaluating result for 'HumanEval/0' task
            "base_eval":List[bool],                     
            # base_eval;a list storing whether executing results are true or false on the base test case
            "base_num":int,                             
            # base_num;the number of the base test cases
            "base_AC":int,                              
            # base_AC;the number of the base test cases that the code LLM generated passed
            "base_TLE":int,                             
            # base_TLE;the number of the base test cases that the code LLM generated occurred TimeLimitExceeding Error
            "base_accuracy":float,                      
            # base_accuracy;the pass ratio for the base test cases
            "base_exp_time":List[float],                
            # base_exp_time;the standard runtime of standard solution on the base test cases
            "base_actual_time":List[float],             
            # base_actual_time;the runtime of LLM generated solution on the base test cases
            
            "plus_eval":List[bool],                     
            # plus_eval;a list storing whether executing results are true or false on the plus test cases
            "plus_num":int,                             
            # plus_num;the number of the plus test cases
            "plus_AC":int,                              
            # plus_AC;the number of the plus test cases that the code LLM generated passed
            "plus_TLE":int,                             
            # plus_TLE;the number of the plus test cases that the code LLM generated occurred TimeLimitExceeding Error
            "plus_accuracy":float,                      
            # plus_accuracy;the pass ratio for the plus test cases
            "plus_exp_time":List[float],                
            # plus_exp_time;the standard runtime of standard solution on the plus test cases
            "plus_actual_time":List[bool],              
            # plus_actual_time;the runtime of LLM generated solution on the plus test cases
            
            "accuracy":float                            
            # accuracy;the pass ratio for all test cases
        }
    }   
    

```

### code_all

该文件夹下存储了15个大模型对164项代码生成任务生成的原始回复，可能已无用

### code_eval

该文件下存储了大模型生成的代码的测试结果

### code_execute

该模型下存储了大模型生成代码的运行结果

### code_raw

该文件夹下存储了15个大模型对28项代码生成任务的原始回答结果

### code_sanitize

该文件夹下存储了大模型返回结果经过提取可执行代码后的结果

[^注：code_*文件夹下的文件名即为大模型名]

## DataReadExample

```python {"id":"01HX3M6Z51MW68CZ07F0WXHXNG"}

import json,os
from typing import  *
import pandas as pd
data_eval = {'accuracy':{},"pass":{},"AC":{}}
for dir,subdir,files in os.walk('code_eval'):
    for file in files:
        model = file.replace('_eval_results.json','')
        if file.endswith('.json'):
            with open(os.path.join(dir,file)) as f:
                data = json.load(f)
            data_eval['accuracy'][model] = data['percision']['mean']
            data_eval['pass'][model] = data['PASS']
            data_eval['AC'][model] = data['AC']
```

```python {"id":"01HX3M7JCTM24YSHB3853G0WBV"}
pd.DataFrame(data_eval).sort_values(by="AC",ascending=False)
```

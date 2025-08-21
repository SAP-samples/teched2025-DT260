# Getting Started

In this exercise, you will...

## Introduction

1. The screenshots in this document have been taken using group ID **`00`** and system **`HE4`**. 

2. Please note that ADT dialogs and views, as well as Fiori UIs, may change in future releases.

3. You can find a solution for this workshop in the development package **`ZDT260_SOLUTION_5`**, or you can import it from [here](https://github.com/SAP-samples/abap-platform-rap120/) into the relevant system if not yet available.

**Now, let's get started!**

1.	Click here.
<br>![](/exercises/ex0/images/00_00_0010.png)

2.	Insert this code.
``` abap
 DATA(params) = request->get_form_fields(  ).
 READ TABLE params REFERENCE INTO DATA(param) WITH KEY name = 'cmd'.
  IF sy-subrc <> 0.
    response->set_status( i_code = 400
                     i_reason = 'Bad request').
    RETURN.
  ENDIF.
```

## Summary

Now that you have ... 
Continue to - [Exercise 1 - Exercise 1 Description](../ex1/README.md)

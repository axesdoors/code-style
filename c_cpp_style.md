The next rules have been defined taking into account the rules from:

- **MISRA-C**: 2012
- **MISRA-C++**: 2008

If for any reason any of the below rules conflicts with a MISRA rule, this last one prevails.


### Version
Version of C/C++ code use are strongly linked with the development platform. Ingenia repositories cover from very low performance micro-controllers
up to standard desktop PCs. Details about C/C++ version and extensions must be defined on every repository.

### Types
- Use <stdint.h> types whenever is possible.
- if <stdint.h> is not available, basic types must include the size information.

>     unsigned integer /* Not allowed */
>     UINT32 /* Allowed */
>     uint32_t /* Allowed and preferred */

> This is very critical in embedded systems where the size of an integer varies between MCUs.

### Header and source files
- In general, every .c file should have an associated .h file.
- All public constants, macros, and functions prototypes should be defined in the header file.
- Any reference of a header file (*.h) within a header file should be avoided
- All header files should have to define guards to prevent multiple inclusion. The format of the symbol name should be FILE_NAME_H.

>     #ifndef FILE1_H 
> 
>     #define FILE1_H
>     ...
>     #endif /* FILE1_H */

- Header files must not be included in projects as files. They must be linked to project the include path.
- Headers and source files must follow the next template

>     /* File documentation */
>     
>     /* Includes */
>     
>     /* Defines & macros */
>     
>     /* Global variables declaration */
>     
>     /* Local variables declaration (for .c) */
>     
>     /* Local function prototypes declaration(for .c) */
>     
>     /* Global Function declaration / definition */
>     
>     /* Local function definition (for .c) */

### Scoping

- Avoid static and global variables. Sometimes there's no alternative, but if there is an alternative, then use it, no matter how much effort it involves.
- All local variables must be defined at the top of the C file and declared as static.
- Avoid the use of global variables. Usually, the use of global variables means an incorrect design of the code architecture in modularity terms.
- If a variable could be modified from two different contexts, as in multithreading applications or in code with interrupts, it should be declared as volatile.
- A function variable should be placed in the narrowest possible scope.
- All local functions must be defined in the C file and declared as static at the top of the file.
- All global functions must be defined in the C file and declared in the header file (*.h).
- Local Macros should be declared in C file.
- Global macros should be declared in header file (*.h).

### Classes

- Declare a class's public section first. Any protected items come next, and then private ones. The order of items should roughly correspond to how likely a reader
is to be interested in them, so the private implementation details come last.
- For simple, short inline classes, especially ones inside a cpp file that are only used locally for a limited purpose, they should probably use struct rather than
class since you’re generally going to have all public members, and this will save a line of code doing the initial public:
- The layout for inherited classes is to put them to the right of the class name, vertically aligned. We leave a single space before the class name, and 2-3 spaces
after the class name before the colon, e.g.

>     class Thing  : public Foo,
>                    private Bar
>     {

- Always include the public/private/protected keyword for each inherited class
- Put a class's member variables (which should almost always be private, of course), after all the public and protected method declarations.
- Any private methods should go towards the end of the class, after the member variables.
- Constructors that take a single parameter should usually be marked explicit. Obviously there are cases where you do want implicit conversion, but always think
about it carefully before writing a non-explicit constructor.

### Loops
- C for loops must declare the iterating variables at beggining of the loop

>    for (uint8_t u8i = (uint8_t)0U; u8i <= (uint8_t)3U; u8i++)
>    {
>        
>    }

### Naming
- Filenames should be all lowercase and can include optionally underscores '_'. 
- Function names are also written in camel-case, but always begin with a capital letter.

>     int16_t
>     GetDataFile(void);
>       
>     int32_t
>     ComputeRectangleArea(int32_t i32Width, int32_t i32Height);


- Class names are also written in camel-case, but always begin with a capital letter, e.g. MyClassName
- Namespaces are also written in camel-case, but always begin with a capital letter. e.g. MyClassName
- In C language, namespaces can be simulated by adding a prefix + underscore before the function / variable.
- Leading or trailing underscores are never allowed in variable names! Leading underscores have a special status for use in standard library code,
so to use them in use code looks quite jarring.
- If you really have to write a macro, it must be ALL_CAPS_WITH_UNDERSCORES. As they’re the only symbols written in all-caps, this makes them easy to spot.
- Since macros have no namespaces, their names must be guaranteed not to clash with macros or symbols used in other libraries or 3rd party code, so you should
start them with something unique to your project.
- Macros should be aligned between them to facilitate readability.

>     #define MAX_ITERATIONS 25
>     #define MIN_VALUE(X, Y)   ((X) < (Y) ? (X) : (Y))

- Hungarian notation must be used. 
- Variables names should start with the data type and have a capital letter for each new word (camel-case). No underscores are allowed
>     int16_t i16SavingAccount;
>     uint8_t u8Line;
>     /* booleans can start with 'is' or with 'b' */
>     bool isInterruptEnabled;
>     bool bInterruptEnabled;
>     char cMyChar;


- Non-standard data type names start with a 'T' capital letter and have a capital letter for each new word, with no underscores:

>     typedef struct TLineDefinition;
>     typedef struct
>     {
>         uint8_t u8ConnFmat;
>         int16_t i16NodeCtrl;
>     }THeader;

- Enumerates types should start with an 'E' capital letter and have a capital letter for each new word, with no underscores: 

>     typedef enum 
>     {
>         ERROR_OVERCURRENT = 0,
>         ERROR_OVERVOLTAGE
>     }EErrorCodes;

- Union types should start with an 'U' capital letter and variables inside should be camel case, without underscores:

>     typedef union 
>     {
>         uint16_t u16Word;
>         uint32_t u32DWord;
>         floatfFloat;
>     }UMsg;

- The variables from an enumeration, union or non-standard data type would be:

>     TLineDefinition tNewLine;
>     EErrorCodes eErrCode;
>     UMsg uRxMsg;

- To simplify its identification, pointer variables must start with a  'p' lower case letter.

>     INT16* pi16Pointer;

### Comments

- All files must contain a brief description about the functionality.

>     /**
>      * @file FileName.c
>      * @brief Description of the main objective of this file
>      */ 

- In order to generate an easy to read documentation, the files should be grouped based on their functionality.
The grouping is done on header files and are used to give to the developer an overview of the hierarchy of the project.

> All groups must be composed by an ID and a "public label". On doxygen, the first word after the @addtogroup command is considered
the group ID, and the others the "public label"

>     /**
>      * @file FileName.h
>      * @brief Description of the main objective of this file
>      * 
>      * @addtogroup MainGroup MainGroup Label
>      * @{
>      *         @addtogroup Subgroup SubGroup Label
>      *         @{
>      */ 
>      
>     Code
>      
>     /**
>      *         @}
>      * @}
>      */
>      
>     END OF FILE

- In this example, if the developer want to do a reference to the group MainGroup, it has to use the ID MainGroup.
However, on the output documentation, the users will read "MainGroup Label"
- Comments must be written using Doxygen tags using the /** prefix, the /*! prefix is forbidden.
- Comments of private or static objects should be done using Doxygen but it is not mandatory.
- Use the following general Doxygen template:

>     /**
>      * @brief   A brief description, one sentence, one line.
>      * @details A detailed description of the functionality, it can span over
>      *          multiple lines, can be omitted.
>      * @pre Prerequisites about the use of the functionality, there can be
>      *      more than one "pre" tags, can be omitted.
>      * @post Postrequisites about the use of the functionality, there can be
>      *       more than one "post" tags, can be omitted.
>      * @noteThere can be one or more notes, can be omitted.
>      *
>      * @param p1description of parameter one
>      * @param p2   description of parameter two
>      * @param p3description of parameter three
>      *
>      * @return  Description of the returned value, must be omitted if
>      *          a function returns void.
>      */ 

- There must be one space after the /*.
- Comments always start with an upper case.
- Comments are always terminated with a dot.

### Formatting
- Only one statement per line is allowed.
- Indentation should be done using 4 spaces.
- TAB characters are completely forbidden.
- End Of Line (EOL) must be CRLF which corresponds to Windows convention.
- Always put a space before and after all binary operators! 
>     x = 1+y - 2*z / 3; /* Bad */
>     x = 1 + y - 2 * z / 3; /* Good */


- The ! operator should always be followed by a space, e.g. if (! foo)
- The ~ operator should be preceded by a space, but not followed by one.
- The ++ and -- operators should have no spaces between the operator and its operand.
- Never put a space before a comma.
- Always put a space after a comma.

>     foo (x,y); /* No */
>     foo (x ,y); /* No */
>     foo (x , y); /* No */
>     foo (x, y); /* Yes */

- Never put a space before an open parenthesis that contains text

>     foo(123);

- Never put a space before an empty pair of open/close parenthesis 

>     foo();

- Don’t put a space before an open square bracket when used as an array index - e.g. foo[1]
- Leave a blank line before if, for, while, do statements when they’re preceded by another 

>     statement. E.g.
>     {
>         uint16_t u16Xyz = (uint16_t)123U;
>        
>         if (u16Xyz != (uint16_t)0U)
>         {
>             foo();
>         }
>        
>         foobar();
>     }

- In general, leave a blank line after a closing brace } (unless the next line is just another close-brace)
- Do not write if statements all-on-one-line…
- Always using braces around if-statements, even very short, simple ones.
- When writing a pointer or reference type, we always put a space after it, and never before it. e.g.

>     SomeObject* myObject = getAPointer();
>     SomeObject& myObject = getAReference();

- When splitting expressions containing operators (or the dot operator) across multiple lines each new line should begin
with the operator symbol.

>     auto xyz = foo + bar /* This makes it clear at a glance */
>                 + func(123) /* that all the lines must be continuations of */
>                 - def + 4321; /* the preceding ones. */
>     
>     auto xyz = foo + bar + /* Not so good.. It takes more effort here */
>                func(123) - /* to see that "func" here is actually part */
>                def + 4321; /* of the previous line and not a new statement. */
>     
>     /* Good: */
>     auto t = AffineTransform::translation(x, y)
>                              .scaled(2.0f)
>                              .rotated(0.5f);
>     
>     /* Bad: */
>     auto t = AffineTransform::translation(x, y).
>         scaled(2.0f).
>        rotated(0.5f);

- The general rule for the length line is max 80 characters. However it is not a strict rule, it must be always prioritized
the visibility of the code in front of this rule.

- Comments has to use /* */.
- Always leave a space before the text in a single line /* comment */
- Hexadecimal is usually written in capital letter 

>     0xABCDEF

- Integer literals & explicit cast: Literal and explicit cast must be used. Take into account the architecture you are using
in order to use the long literal.

>     (uint16_t)1U /* yes! */
>     (int16_t)1 /* yes! */
>     (uint32_t)1UL /* yes! if 32 bits is considered long in the device architecture */
>     (int32_t)1L /* yes! if 32 bits is considered long in the device architecture */

- Floating point literals & explicit cast: We always add at least one digit before and after the dot, e.g. Furthermore,
literal and explicit cast must be used for constants.

>     0.0 /* no! */
>     (float)0.0f /* yes! */
>     0. /* no! */
>     0.f /* no! */
>     .1 /* no! */
>     .1f /* no! */

- In the function definition, the return type of a function should be placed in a different line of the function.
- The function parameters should be on the same function line if they fit; otherwise, wrap arguments at the parenthesis.

>     int32_t
>     ComputeActualVelocity(int32_t i32ActualPosition, int32_ti32LastPosition);
>      
>     int32_t
>     ComputeRegressionLine(int32_t* pi32DataArrayX, int32_t* pi32DataArrayY, 
>                           int32_t* pi32Result, int32_t* pi32Variance);

- Functions without any parameters must use the keyword void.

>     int32_t
>     MyFunction(void);


**Braces are indented based on the Allman style with some modifications**

- All (for, while, if) statements must be written by placing a bracket at the next line of the statement definition line.
The last line must close the statement with another bracket at the same indentation level as the statement definition line.
Brackets should be used even if the statement contains only one instruction.

>     if (u16ElapsedTime > SOME_THRESHOLD)
>     {
>         i16Counter++;
>     }
    
- In case of using else if / else statement, it will be placed in the next line of the bracket.
- In case of checking multiple conditions, each one must be enclosed in parenthesis.

>     if (bCondition == FALSE)
>     {
>         /* Process A */
>     }
>     else
>     {
>         /* Process B */
>     }
>     
>     if ((u8PinMapping[u8Pin] == INPUT) || (u8PinMapping[u8Pin] == OUTPUT))
>     {
>         /* Something */
>     }
>     

- All boolean conditions must be explicitly checked.
- All boolean conditions are compared with the false state.
     
>     bool isInterruptEnabled;
>     
>     if (isInterruptEnabled == false)
>     {
>         
>     }
>     else
>     {
>     
>     }
>     
>     or
>     
>     if (isInterruptEnabled != false)
>     {
>         
>     }
>     else
>     {
>     
>     }

- Default case must be always present when using switch.
- A case could finish without a break to allow falling through another case but it should be clearly documented as in
the following example. A non-empty case is not allowed to end without a break (fallthrough not allowed).
If it's empty it has to be clearly documented also.

>     switch (...)
>     {
>         case SOME_CASE_1:
>               /* Fallthrough */
>         case SOME_CASE_2:
>               /* Code */
>               break;
>         default:
>               /* Nothing */
>               break;
>     }
>     

- A single space must follow the statements: if, while, do and switch before the expression opening.

>     if (i16Value == (int16_t)10)
>     {
>         i16Data = i16Value + (int16_t)5;
>     }

- Functions calls should not include spaces on parenthesis. After a comma space should be always placed.

>     Function1(i16Value1, i16Value2);

- After a type cast should not be an space.

>     i16Value1 = (INT16)i32Value2;


- In pointer declarations, no space should be left between the type and the operator

>     INT16* pi16Value;

- Declarations of a pointer to pointer types should leave no whitespaces between asterisk operators.

>     INT16** pi16Value;

- All preprocessor conditionals must have a comment in the #endif, so it can be easily identified where they come from.

>     #ifndef FILE1_H 
>     #define FILE1_H
>     
>     #if NUM_OF_FDBCKS > 1
>     ...
>     #elif NUM_OF_FDBCKS > 2
>     ...
>     #endif /* NUM_OF_FDBCKS */
>     
>     #endif /* FILE1_H */


### Exceptions to the rules

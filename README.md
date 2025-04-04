## 🖌️ Simple Coding Style
---
My own personal simple coding style for all languages. C++ for demo purposes.

This is the style I used for all my personal repositories. And also for other people to reference 
if needed.

The intention of this coding style is just to keep things __simple and natural__. 

__No cryptic prefixes, suffixes and underscores__ that are specific to different languages.

|

This style just utilizes different case types and English grammar to differentiate different 
entities and intentions.

Here are the cases that are used in this coding style:
- PascalCase: `ThisIsAnExampleOfPascalCase`
- camelCase: `thisIsAnExampleOfCamelCase`
- MACRO_CASE: `THIS_IS_AN_EXAMPLE_OF_MACRO_CASE`

### ✏️ Identifiers / Names
---
Identifiers or names should require minimum context to understand their purpose or what they store. 
Therefore, abbreviations should be avoided as it requires context to understand what it stands for. 

Try to be __simple and explicit__ (and sometimes long) rather than cryptic and short. 

- #### Anything (enum value, variables, etc...) constant or macro - MACRO_CASE
    - **Explanation:** Macro case is used in many places that denote __constants__ and __macros__ 
    due to historic reasons. This can easily tell that something is __CONSTANT__ and 
    __you can't change it__.
    - Example: `static const int NUM_OF_LEGS_FOR_HUMANOID = 2;`
- #### Anything lives inside a function - camelCase
    - **Explanation:** This can easily differentiate a __local variable__ in a function against 
    class or global variables and is especially helpful in a relatively long function.
        ```c++
            //Example
            float NormalizedSum(int* vec, size_t count, int min, int max)
            {
                int sum = 0;
                for(int i = 0; i < count; i++)
                    sum += vec[i]
            
                float normalizeRange = max - min;
                return (float)(sum - min) / normalizeRange;
            }    
        ```
- #### Anything that can be accessed outside a function - PascalCase
    - **Explanation:** Clean, easy to read without any cryptic underscores or prefixes/suffixes, 
    and can clearly indicate it can be used/accessed by other functions.
    - __Types Names or Class/Namespace Identifier - Explicit Noun__
        - Example: `class ModulesManager`
    - __Class/Global variables names - Explicit Noun Or State__
        - Example: `bool Initialized;`
        - 
        - Normally, class variable name should tell its purpose and therefore be different from 
        its type identifier.
          However, if this is not possible, simply prepending a word like __`Current`__ will do the 
          job to tell it is not a type.
        - Example: Type `ModulesManager` and class variable `CurrentModulesManager` 
    - __Function names - Explicit Verb, Question or Query__
        - Example: Function `void CheckStatus()` or `bool IsStatusChecked()`.
    - Example:
        ```c++
            //Example
            #ifndef HUMANOID_ENEMY_HPP
            #define HUMANOID_ENEMY_HPP
            namespace MyGame
            {
                class HumanoidEnemy
                {
                    private:
                        bool Initialized;
                        ModulesManager CurrentModulesManager;
                        bool StatusChecked;
                        static const int NUM_OF_LEGS_FOR_HUMANOID = 2;
                    public:
                        HumanoidEnemy();
                        ~HumanoidEnemy();
                        void Initialize();
                        void CreateModule(std::string moduleName);
                        void RemoveModule(std::string moduleName);
                        void CheckStatus();
                        bool IsStatusChecked();
                };
            }
            #endif
        ```

### 📐 Formatting
---
- #### Auto Formatter - No
    - **Explanation:** This is because it requires quite some effort to setup and is often not 
    flexible enough. 
    - If a formatter is required, Microsoft style with slight modification suits the best.
- #### Indentations - 4 spaces and indent for every section/block if possible
    - **Explanation:**
        - Allow differentiating __blocks/sections of code easily__
        - Spaces allow __easier manual formatting__
        - Guarantee to look the same __across machines and websites__.
        - You can configure on most of the modern IDE or text editor to align and output 4 
        spaces quite easily.
- #### Bracket style - Newline and same indentation
    - **Explanation:** Easier to read by providing spacing against the block content. Expecially
    when dealing multi-line if statement where the bracket in newline helps differentiate the if
    statement lines and the actual content lines.
    - Example:
        ```
            if( HasSomethingHappened() ||
                HasSomethingElseHappened() ||
                HasSomeOtherThingsHappened())
            {
                DoSomethingUrgent(a, b);
                DoSomethingNext();
            }
            else
            {
                DoSomethingNotUrgent(a, b);
                DoSomethingElseNext();
            }

            //Instead of

            if( HasSomethingHappened() ||
                HasSomethingElseHappened() ||
                HasSomeOtherThingsHappened()) {
                DoSomethingUrgent(a, b);
                DoSomethingNext();
            } else {
                DoSomethingNotUrgent(a, b);
                DoSomethingElseNext();
            }
        ```
    - The first one looks perfectly spaced and logical, while the second one looks clustered and
    difficult to differentiate if statements and the actual execution statements
    - **Explanation:** Makes commenting and temporary change much easier to do
    - Example:
        ```c++
            if(condition())
            {
                //Many statements here
            }
            
            //Can be commented out to skip the condition easily like this
            
            //if(condition())
            {
                //Many statements here
            }
            
            //Or to temporary disable a block but still have it being compiled
            
            //if(condition())
            if(false)
            {
                //Many statements here
            }
- #### Nested Functions such as Lambda - Treat as new block
    - **Explanation:** Nested functions are **functions** and should be treated and formatted 
    like so to be **consistent**.
    -   Example: 
        ```c++
            void SomeFunction()
            {
                auto someLambda = []()
                {
                    //...
                }
                
                //Or move the [...](...) to its own line if it gets too long, but indented
                auto someLongLambda = 
                [&MyVar, &MyVar2, &MyVar3, ...](int myParam, int myParam2, int myParam3, ...)
                {
                    
                }
                
                //Or like this (See Multilines section)
                auto someLongLambda = 
                [
                    &MyVar, 
                    &MyVar2, 
                    &MyVar3, 
                    ...
                ]
                (
                    int myParam, 
                    int myParam2, 
                    int myParam3, 
                    ...
                )
                {
                    ...
                }
            }
    
        ```
- #### Width/Column Limit - 100 characters
    - **Explanation:**
        - 100 characters hard limit allows the source file to be limited to half of the width of 
        any display, which...
        - Allows __side by side__ header and source files
        - If a line is too long, it should be __spread across multiple lines by parameters or 
        member action.__ (See Multilines section)
- #### Multilines
    - Try to align with nearest tab with parameters or statements. 
        - **Explanation:** Aligning with nearest tabs makes manual formatting a lot quicker, 
        it's a good compromise.
    - For assignment, you can also split right half (i.e. after `=`) to next line indented if 
    doesn't fit single line. 
        - **Explanation:** Splitting assignment to 2 lines when needed is also readable and easy
        to format.
    - If the above 2 is not possible, format the parameters by blocks like a function block
        - **Explanation:** This is a last resort and should be used sparingly. It looks slightly
        weird but it is consistent with how lambdas and how function is declared. This is 
        particularly useful when used in an deeply nested indented block.
    - Long Assignment
        ```c++
            MyClass myVariable = 
                someVariable.FirstFunction(mySuperDuperLongArgName, mySuperDuperLongArgName2);
            
            //Or
            
            MyClass myVariable = someVariable.FirstFunction(mySuperDuperLongArgName, 
                                                            mySuperDuperLongArgName2);
        ```
    - Function:
        ```c++
                                                //Align variables to the nearest tab width
            void FunctionWithLotsOfParameters(  int a,
                                                bool b,
                                                std::string c,
                                                std::vector& someVector,
                                                //A few more parameters...
                                                uint32_t finalParameter)
            {
                //...
            }
        ```
    - Long if
        ```c++
                //Align to nearest tab width
            if( (ConditionFunction(var1, var2, var3) && 
                conditionVariable && 
                conditionVariable2) ||
                conditionVariable3)
            {
                DoSomething();
            }
        ```
    - Long Lambda
        ```c++
            auto someLongLambda = 
            [
                &MyVar, 
                &MyVar2, 
                &MyVar3, 
                ...
            ]
            (
                int myParam, 
                int myParam2, 
                int myParam3, 
                ...
            )
            {
                ...
            }
        ```
    - Constructor
        ```c++
        ```
    - Function Call With Many/Long Arguments
        ```c++
                                //Align to nearest tab width
            SomeFunctionCall(   MyArgWithLongName1, 
                                MyArgWithLongName2, 
                                MyArgWithLongName3,
                                MyArgWithLongName4);
            
            //Or in a very nested indented situation
            
            bool MyFunction(...)
            {
                for(...)
                {
                    if(...)
                    {
                        for(...)
                        {
                            MY_MACRO_FOR_HANDLING_ERROR
                            (
                                SomeFunctionCall
                                (
                                    MyArgWithLongName1, 
                                    MyArgWithLongName2, 
                                    MyArgWithLongName3,
                                    MyArgWithLongName4
                                )
                            );
                        }
                    }
                }
                
                ...
            }
        ```
    - Function Chaining
        -   Align with `.`/`->` operator
            ```c++
                            //Align to nearest tab width
                someVariable.FirstFunction()
                            .SecondFunction()
                            .ThirdFunction();
            ```
        - Align with block if unable to align with `.`/`->` operator
            ```c++
                                    //Align to nearest tab width
                FunctionInSameScope (var1, var2, var3, var4, var5, ...)
                                    .SecondFunction()
                                    .ThirdFunction(var1, var2, ...);
            ```
- #### Omitting Bracket - Only when the condition and action are short
    - Saves a few lines, quicker and easier to read if an empty line is there to separate each block.
      While it is possible to misindent multiple actions, it rarely happens to me 
      and compilers are smart enough to give warnings.
        ```c++
            if(!IsWaterBoiled())
                BoilWater();

            if(!HasChocolatePowder())
                GetTeaBag();
            else
                GetChocolatePowder();

            CreateBeverage();
        ```

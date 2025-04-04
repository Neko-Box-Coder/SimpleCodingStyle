## 🖌️ Simple Coding Style
---
My own personal simple coding style for C++, but most of them can be apply to many languages.

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

### 🗿 Architecture and Philosophy
---

This section applies to most languages, not just c++

#### Keep it simple, stupid
Things are always changing, new deadlines, new features, new requirements.

Instead of spending time over-engineering and designing something that you don't know how it will 
work out (and you never will, unless you can predict the future), get it working first. After that
you can do whatever you want, refactor it, clean it, etc.

Nothing is future proof and don't waste your time trying to pretend you will design something 
future proof. Something "good enough" is often the right balance between how much time is spent on
designing the architecture and implementation to allow changes if needed.

#### Priority readability and iterability over pre-mature optimization
This of course is different per context/scenarios, but generally languages are fast enough under 
most use-cases, and performance often only matters under certain part of a program (i.e. a hot loop).

Sacrificing readability and iterability (i.e. using an array/struct for better CPU caching over 
(linked) list strurcture that reads better) on non performance critical part isn't worth it.

That is not to say you should make it inefficient, small habits like adding `const` and using 
reference is generally good to do and doesn't hurt readability.

A good rule of thumb is "Make it easy to read first, then make it run faster"

#### No Inheritance, No OOP, No Interface, No "Clean Code", No "SOLID"
The mainstream really endorse OOP. Schools, corporates, books are all praising it because it is 
"Battle Tested" and is the industry de facto, not knowing how painfully confusing to navigate 
and difficult to add features to these codebase. Not to mention it is slower as well, see 
["Clean" Code, Horrible Performance](https://www.youtube.com/watch?v=tD5NrevFtbU) by Casey Muratori.

Here by OOP I mean **object inheritance and interfaces**, not functions that are binded to 
a data structure, not RAII.

OOP sounds great on paper, you re-use things with hierarchical design, isolation of concerns and 
allow easy testing. But in practice it often falls short in terms of readability and iterability.

Trying to follow through a flow on what a function does? Too bad you will have to go to 20 
different files to understand which concrete functions **can** be called, not to mention there
can be **more than 1** concrete functions can be called if it is a multi-layer inheritance. 

Using interfaces to make it easier for testing? Good luck diving through many layers and wrappers 
when you are trying to debug or read the code, only to find out the logic is a dynamic 
delegate/function object that is spawned and configured somewhere else and you now have to find 
where that is. Also good luck adding new features since the moment you change the interface, 
you are forced to update 20 other classes (and tests) that are dependent on that interface.

Of course "Clean Code" and "SOLID" says none of these are an issue if you follow their rules.
This will be true if we live in a perfect world where there are no deadlines, no change of 
requirements, no unique new features, no special use cases from the customer, etc. 

But we don't live in a perfect world, this is just a lie so that people can sell books and host 
talks and the only people can do this are big corporations which can afford to this and the rest
of us are wasting time trying to perfect something that is always imperfect. 

I have been to [this](https://github.com/Neko-Box-Coder/ssGUI) 
[path](https://github.com/Neko-Box-Coder/CppOverride) (Using OOP) and **DEEPLY** regretted it. 

Just implement the damn thing. 

#### Composition, If and Switch Statements

Instead of OOP and all those shit, just use composition, good ol' if statements and switch 
statements when you need to generalize something. Not only it is easier to read since now each 
function in the generalization shows **EXACTLY** what concrete functions it will call, it is easier
to debug as well since you can can access all the concrete data just by poking into the member 
data structure.

#### Explicit abstractions over implicit abstractions, no need to hide implementations

TODO...

#### Simple, explicit code leads to readability


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
    - **Explanation:** Easier to read by providing spacing against the block content. Especially
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
            //Normal case
                                                        //Align to nearest tab width
            runcpp2::NodeRequirement::NodeRequirement(  const std::string& name, 
                                                        ryml::NodeType nodeType, 
                                                        bool required,
                                                        bool nullable) :    Name(name), 
                                                                            NodeType(nodeType), 
                                                                            Required(required), 
                                                                            Nullable(nullable)
            {
                ...
            }
            
            //Extreme case
                                            //Align to nearest tab width
            HumanoidEnemy::HumanoidEnemy(   int myParam1, 
                                            const std::unordered_map<std::string, int>& myParam2, 
                                            std::unordered_map< std::string, 
                                                                std::vector<MyObject>>& myParam3,
                                            const std::unordered_map
                                            <
                                                std::string, 
                                                std::vector<const MyObject&>
                                            >& myParam4,
                                            std::vector<std::pair<int, bool>> myParam5) :
                //Move to indented newline since there is no space after :
                MyMember1(myParam1),
                MyMember2(myParam2),
                MyMember3(myParam3),
                MyMember4(myParam4),
                MyMember5(myParam5)
            {
                ...
            }
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

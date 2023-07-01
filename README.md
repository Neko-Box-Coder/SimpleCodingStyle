## 🖌️ NekoBoxCoder Simple Coding Style
---
My own personal simple coding style for all languages. C++ for demo purposes.

This is the style I used for all my personal repositories. And also for other people to reference if needed.

The intention of this coding style is just to keep things __simple and natural__. 

__No cryptic prefixes, suffixes and underscores__ that are specific to different languages.

|

This style just utilizes different case types and English grammar to differentiate different entities and intentions.

Here are the cases that are used in this coding style:
- PascalCase: `ThisIsAnExampleOfPascalCase`
- camelCase: `thisIsAnExampleOfCamelCase`
- MACRO_CASE: `THIS_IS_AN_EXAMPLE_OF_MACRO_CASE`

### 📐 Formatting
---
- #### Auto Formatter - Normally no as it requires quite some effort to setup and is often not flexible enough.
- #### Indentations - 4 spaces and indent for every section/block if possible
    - Allow differentiating __blocks/sections of code easily__
    - Spaces allow __easier manual formatting__
    - looks more or less __uniform across devices__.
    - You can configure on most of the modern IDE or text editor to align and output 4 spaces quite easily.
- #### Width Limit - Soft limit of around 60% of monitor width 
    - Width limit is __quite situational__
    - Allows __side by side__ header and source files
    - When it is not too long, I will normally leave it on the same line. Otherwise, I would __spread it across multiple lines by parameters.__
    - ```c++
        void FunctionWithAFewParameters(int a, bool b, std::string c)
        {
            //....
        }
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
- #### Bracket style - Newline and same indentation
    - Easier to read by providing spacing against the block content
    - ```
        if(HasSomethingHappened())
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

        if(HasSomethingHappened()) {
            DoSomethingUrgent(a, b);
            DoSomethingNext();
        } else {
            DoSomethingNotUrgent(a, b);
            DoSomethingElseNext();
        }
      ```
    - The first one looks perfectly spaced and logical, while the second one looks quite clustered
- #### Omitting Bracket - Only when the condition and action are short
    - Saves a few lines, quicker and easier to read if an empty line is there to separate each block.
      While it is possible to misindent multiple actions, it rarely happens to me and compilers are smart enough to give warnings.
    - ```c++
        if(!IsWaterBoiled())
            BoilWater();

        if(!HasChocolatePowder())
            GetTeaBag();
        else
            GetChocolatePowder();

        CreateBeverage();
      ```

### ✏️ Identifiers / Names
---
Identifiers or names should require minimum context to understand their purpose or what they store. Therefore, abbreviations should be avoided as it requires context to understand what it stands for. 

Try to be __simple and explicit__ (and sometimes long) rather than cryptic and short. 

- #### Anything (enum value, variables, etc...) constant or macro - MACRO_CASE
    - Macro case is used in many places that denote __constants__ and __macros__ due to historic reasons. This can easily tell that something is __CONSTANT__ and __you can't change it__.
    - Example: `static const int NUM_OF_LEGS_FOR_HUMANOID = 2;`
- #### Anything lives inside a function - camelCase
    - This can easily differentiate a __local variable__ in a function against class or global variables and is especially helpful in a relatively long function.
    - ```c++
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
    - Clean, easy to read without any cryptic underscores or prefixes/suffixes, and can clearly indicate it can be used/accessed by other functions.
    - __Types Names or Class/Namespace Identifier - Explicit Noun__
        - Example: `class ModulesManager`
    - __Class/Global variables names - Explicit Noun Or State__
        - Example: `bool Initialized;`
        - 
        - Normally, class variable name should tell its purpose and therefore be different from its type identifier.
          However, if this is not possible, simply prepending a word like __`Current`__ will do the job to tell it is not a type.
        - Example: Type `ModulesManager` and class variable `CurrentModulesManager` 
    - __Function names - Explicit Verb, Question or Query__
        - Example: Function `void CheckStatus()` or `bool IsStatusChecked()` and class variable `bool StatusChecked`.
        -
    - ```c++
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








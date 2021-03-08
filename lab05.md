


# Lab 5

## Steps 1 & 2:

#### Tutorial.cxx:
``` C
/ A simple program that computes the square root of a number
#include <cmath>
#include <iostream>
#include <string>
#include "TutorialConfig.h"

#ifdef USE_MYMATH
#include "MathFunctions.h"
#endif

int main(int argc, char* argv[])
{
  if (argc < 2) {
    // report version
    std::cout << argv[0] << " Version " << Tutorial_VERSION_MAJOR << "."
              << Tutorial_VERSION_MINOR << std::endl;
    std::cout << "Usage: " << argv[0] << " number" << std::endl;
    return 1;
  }

  // convert input to double
const double inputValue = std::stod(argv[1]);

#ifdef USE_MYMATH
  const double outputValue = mysqrt( inputValue );
#else
  const double outputValue = sqrt( inputValue );
#endif
  // calculate square root
  std::cout << "The square root of " << inputValue << " is " << outputValue
            << std::endl;
  return 0;
}
```



#### CMakeLists.txt:
```CMake
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

option( USE_MYMATH "Use tutorial provided math implementation" ON )

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

if( USE_MYMATH )
        add_subdirectory( MathFunctions )
        list( APPEND EXTRA_LIBS MathFunctions )
        list( APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctions" )
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)
target_link_libraries( Tutorial PUBLIC ${EXTRA_LIBS} )

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           ${EXTRA_INCLUDES}
                          )


```
#### Screen Shot:
![image](https://user-images.githubusercontent.com/40222287/110222650-c1baa080-7ea1-11eb-99c1-f5ab50ce2837.png)



## Step 3:

#### CMakeLists.txt:
``` CMake
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# should we use our own math functions
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

# add the MathFunctions library
if(USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )
```


#### MathFunctions/CmakeLists.txt:
``` CMake
add_library(MathFunctions mysqrt.cxx )

target_include_directories( MathFunctions
                INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
                )
```


#### Screen Shot:

![image](https://user-images.githubusercontent.com/40222287/110222950-47d7e680-7ea4-11eb-9432-ab301c159e7f.png)





## Step 4:

#### CMakeLists.txt:
``` CMake
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# should we use our own math functions
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

# add the MathFunctions library
if(USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )


install( TARGETS Tutorial DESTINATION bin )
install( FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h"
        DESTINATION include
        )


enable_testing()

add_test( NAME Runs COMMAND Tutorial 25 )

add_test( NAME Usage COMMAND Tutorial )
add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )


install( TARGETS Tutorial DESTINATION bin )
install( FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h"
        DESTINATION include
        )

enable_testing()

add_test( NAME Runs COMMAND Tutorial 25 )

add_test( NAME Usage COMMAND Tutorial )

```



#### MathFunctions/CmakeLists.txt:
```CMake
add_library(MathFunctions mysqrt.cxx)

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
          )

  install( TARGETS MathFunctions DESTINATION lib )
  install( FILES MathFunctions.h DESTINATION include )

```


#### Screen Shot:
![image](https://user-images.githubusercontent.com/40222287/110373441-7e853c80-801d-11eb-9cb7-99e8198b316d.png)




## Step 5:


#### CMakeLists.txt:
``` CMake


```



#### MathFunctions/CmakeLists.txt:
```CMake

```


#### Screen Shot:



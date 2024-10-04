cmake_minimum_required(VERSION 3.10)

project(problems)

# List of example folder (add more if needed)
set(EXAMPLES_LIST
    hello-world
    week01-dominoes
    # Add more folders here ...
)

# Build for all examples
foreach(example ${EXAMPLES_LIST})
    add_subdirectory(${example})
endforeach()
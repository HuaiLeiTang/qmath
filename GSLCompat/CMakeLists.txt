
add_library(QMathGSL GSLCompat.cpp)

install (TARGETS QMathGSL DESTINATION bin)
install (FILES GSLCompat.hpp DESTINATION include)



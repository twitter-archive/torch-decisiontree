SET(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})

CMAKE_MINIMUM_REQUIRED(VERSION 2.6 FATAL_ERROR)
CMAKE_POLICY(VERSION 2.6)

FIND_PACKAGE(Torch REQUIRED)

SET(CMAKE_C_FLAGS "--std=c99 -pedantic -Werror -Wall -Wextra -Wno-unused-function -D_GNU_SOURCE ${CMAKE_C_FLAGS}")
SET(src
   init.c
   hash_map.c
)
SET(luasrc
   _env.lua
   benchmark.lua
   CartNode.lua
   CartTrainer.lua
   CartTree.lua
   DataSet.lua
   DecisionForest.lua
   DecisionForestTrainer.lua
   DecisionTree.lua
   DFD.lua
   GiniState.lua
   GradientBoostState.lua
   GradientBoostTrainer.lua
   init.lua
   LogitBoostCriterion.lua
   math.lua
   MSECriterion.lua
   RandomForestTrainer.lua
   Sparse2Dense.lua
   SparseTensor.lua
   test.lua
   TreeState.lua
   utils.lua
   WorkPool.lua
)

ADD_TORCH_PACKAGE(decisiontree "${src}" "${luasrc}" "A decision tree library, for Torch")

TARGET_LINK_LIBRARIES(decisiontree luaT TH)

IF (BUILD_STATIC OR "$ENV{STATIC_TH}" STREQUAL "YES")
   MESSAGE(building decisiontree statically)
   SET_TARGET_PROPERTIES(decisiontree_static PROPERTIES COMPILE_FLAGS "-fPIC -DSTATIC_TH")
ENDIF()

INSTALL(FILES ${luasrc} DESTINATION "${Torch_INSTALL_LUA_PATH_SUBDIR}/decisiontree")

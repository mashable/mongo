add_library(bson_template_evaluator STATIC
  bson_template_evaluator
  )
add_dependencies(bson_template_evaluator generate_error_codes generate_action_types)
target_link_libraries(bson_template_evaluator
  LINK_PUBLIC
  bson
  )

if (JS_ENGINE STREQUAL v8)
  include_directories(${V8_INCLUDE_DIRS})
  add_library(scripting STATIC
    engine
    utils
    bench
    engine_v8
    v8_db
    v8_utils
    v8_profiler
    )
  add_dependencies(scripting generate_error_codes generate_action_types)
  target_link_libraries(scripting LINK_PUBLIC
    bson_template_evaluator
    ${V8_LIBRARIES}
    mongocommon
    stringutils
    )
elseif (JS_ENGINE STREQUAL sm)
  message(SEND_ERROR "spidermonkey not supported with cmake")
else ()
  message(SEND_ERROR "unknown js engine")
endif ()

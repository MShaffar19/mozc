# -*- coding: utf-8 -*-
# Copyright 2010-2021, Google Inc.
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are
# met:
#
#     * Redistributions of source code must retain the above copyright
# notice, this list of conditions and the following disclaimer.
#     * Redistributions in binary form must reproduce the above
# copyright notice, this list of conditions and the following disclaimer
# in the documentation and/or other materials provided with the
# distribution.
#     * Neither the name of Google Inc. nor the names of its
# contributors may be used to endorse or promote products derived from
# this software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
# "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
# LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
# A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
# OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
# SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
# LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
# DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
# THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
# OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

# Visibility:  please choose a more appropriate default for the package,
# and update any rules that should be different.

load(
    "//:build_defs.bzl",
    "cc_binary_mozc",
    "cc_library_mozc",
    "cc_test_mozc",
    "select_mozc",
)

package(default_visibility = [
    "//:__subpackages__",
])

test_suite(
    name = "android_test",
    tests = [
        ":ime_switch_util_test_android",
        ":key_info_util_test_android",
        ":output_util_test_android",
        ":random_keyevents_generator_test_android",
        ":request_test_util_test_android",
        ":session_converter_stress_test_android",
        ":session_converter_test_android",
        ":session_handler_stress_test_android",
        ":session_handler_test_android",
        ":session_observer_handler_test_android",
        ":session_regression_test_android",
        ":session_test_android",
        ":session_usage_stats_util_test_android",
        # Disabled tests due to errors.
        # ":session_handler_scenario_test_android",
        # ":session_usage_observer_test_android",
    ],
)

cc_library_mozc(
    name = "session_converter",
    srcs = ["session_converter.cc"],
    hdrs = ["session_converter.h"],
    deps = [
        ":session_converter_interface",
        ":session_usage_stats_util",
        "//base",
        "//base:logging",
        "//base:port",
        "//base:text_normalizer",
        "//base:util",
        "//composer",
        "//config:config_handler",
        "//converter:converter_interface",
        "//converter:converter_util",
        "//converter:segments",
        "//protocol:commands_proto",
        "//protocol:config_proto",
        "//request:conversion_request",
        "//session/internal:candidate_list",
        "//session/internal:session_output",
        "//transliteration",
        "//usage_stats",
        "@com_google_absl//absl/flags:flag",
    ],
)

cc_test_mozc(
    name = "session_converter_test",
    size = "small",
    timeout = "moderate",
    srcs = ["session_converter_test.cc"],
    requires_full_emulation = False,
    deps = [
        ":request_test_util",
        ":session_converter",
        "//base",
        "//base:logging",
        "//base:system_util",
        "//base:util",
        "//composer",
        "//composer:table",
        "//config:config_handler",
        "//converter:converter_mock",
        "//converter:segments",
        "//data_manager/testing:mock_data_manager",
        "//protocol:candidates_proto",
        "//protocol:commands_proto",
        "//protocol:config_proto",
        "//session/internal:candidate_list",
        "//session/internal:keymap",
        "//testing:gunit_main",
        "//testing:testing_util",
        "//transliteration",
        "//usage_stats",
        "//usage_stats:usage_stats_testing_util",
        "@com_google_absl//absl/flags:flag",
        "@com_google_absl//absl/memory",
    ],
)

cc_test_mozc(
    name = "session_converter_stress_test",
    size = "large",
    srcs = ["session_converter_stress_test.cc"],
    requires_full_emulation = False,
    deps = [
        ":session_converter",
        "//base",
        "//base:clock",
        "//base:port",
        "//base:system_util",
        "//base:util",
        "//composer",
        "//composer:table",
        "//config:config_handler",
        "//engine:engine_interface",
        "//engine:mock_data_engine_factory",
        "//protocol:commands_proto",
        "//protocol:config_proto",
        "//testing:gunit_main",
        "@com_google_absl//absl/flags:flag",
    ],
)

cc_library_mozc(
    name = "session",
    srcs = [
        "session.cc",
        "session_interface.h",
    ],
    hdrs = ["session.h"],
    visibility = [
        "//:__subpackages__",
    ],
    deps = [
        ":session_converter",
        ":session_usage_stats_util",
        "//base",
        "//base:clock",
        "//base:logging",
        "//base:port",
        "//base:singleton",
        "//base:url",
        "//base:util",
        "//base:version",
        "//composer",
        "//composer:key_event_util",
        "//composer:table",
        "//config:config_handler",
        "//engine:engine_interface",
        "//engine:user_data_manager_interface",
        "//protocol:commands_proto",
        "//protocol:config_proto",
        "//session/internal:ime_context",
        "//session/internal:key_event_transformer",
        "//session/internal:keymap",
        "//session/internal:keymap_factory",
        "//session/internal:session_output",
        "//testing:gunit_prod",
        "//transliteration",
        "//usage_stats",
        "@com_google_absl//absl/memory",
    ],
)

cc_test_mozc(
    name = "session_test",
    size = "large",
    srcs = [
        "session_interface.h",
        "session_test.cc",
    ],
    requires_full_emulation = False,
    deps = [
        ":request_test_util",
        ":session",
        ":session_converter_interface",
        "//base",
        "//base:logging",
        "//base:port",
        "//base:util",
        "//composer",
        "//composer:key_parser",
        "//composer:table",
        "//config:config_handler",
        "//converter:converter_mock",
        "//converter:segments",
        "//data_manager/testing:mock_data_manager",
        "//dictionary:pos_matcher_lib",
        "//engine",
        "//engine:mock_converter_engine",
        "//engine:mock_data_engine_factory",
        "//engine:user_data_manager_mock",
        "//protocol:candidates_proto",
        "//protocol:commands_proto",
        "//protocol:config_proto",
        "//request:conversion_request",
        "//rewriter:transliteration_rewriter",
        "//session/internal:ime_context",
        "//session/internal:keymap",
        "//testing:gunit_main",
        "//testing:mozctest",
        "//usage_stats",
        "//usage_stats:usage_stats_testing_util",
        "@com_google_absl//absl/memory",
    ],
)

cc_test_mozc(
    name = "session_regression_test",
    size = "large",
    srcs = [
        "common.h",
        "session_interface.h",
        "session_regression_test.cc",
    ],
    requires_full_emulation = False,
    deps = [
        ":request_test_util",
        ":session",
        ":session_converter_interface",
        ":session_handler",
        "//base",
        "//base:file_util",
        "//base:logging",
        "//base:port",
        "//base:system_util",
        "//composer:key_parser",
        "//composer:table",
        "//config:config_handler",
        "//converter:segments",
        "//data_manager/testing:mock_data_manager",
        "//engine:mock_data_engine_factory",
        "//engine:user_data_manager_interface",
        "//protocol:candidates_proto",
        "//protocol:commands_proto",
        "//protocol:config_proto",
        "//rewriter:rewriter_interface",
        "//session/internal:ime_context",
        "//session/internal:keymap",
        "//testing:gunit_main",
        "@com_google_absl//absl/flags:flag",
        "@com_google_absl//absl/memory",
    ],
)

cc_library_mozc(
    name = "session_observer_handler",
    srcs = ["session_observer_handler.cc"],
    hdrs = ["session_observer_handler.h"],
    deps = [
        ":session_observer_interface",
        "//base",
        "//base:port",
        "//protocol:commands_proto",
        "//protocol:config_proto",
    ],
)

cc_test_mozc(
    name = "session_observer_handler_test",
    size = "small",
    srcs = ["session_observer_handler_test.cc"],
    requires_full_emulation = False,
    deps = [
        ":session_observer_handler",
        ":session_observer_interface",
        "//protocol:commands_proto",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "session_handler",
    srcs = [
        "common.h",
        "session_handler.cc",
        "session_interface.h",
    ],
    hdrs = ["session_handler.h"],
    deps = select_mozc(
        client = [],
        default = [
            ":session_watch_dog",
            "//base:process",
        ],
        linux = [
            ":session_watch_dog",
            "//base:process",
        ],
        macos = [
            ":session_watch_dog",
            "//base:process",
        ],
    ) + [
        ":session",
        ":session_handler_interface",
        ":session_observer_handler",
        "//base",
        "//base:clock",
        "//base:stopwatch",
        "//base:version",
        "//composer",
        "//config:character_form_manager",
        "//config:config_handler",
        "//converter:converter_interface",
        "//dictionary:user_dictionary_session_handler",
        "//engine:engine_builder_interface",
        "//engine:user_data_manager_interface",
        "//engine:engine_interface",
        "//storage:lru_cache",
        "//usage_stats",
        "@com_google_absl//absl/memory",
    ] + [
        "@com_google_absl//absl/flags:flag",
        "//base:logging",
        "//base:port",
        "//base:singleton",
        "//base:util",
        "//composer:table",
        "//protocol:commands_proto",
        "//protocol:config_proto",
        "//protocol:user_dictionary_storage_proto",
        "//testing:gunit_prod",
    ],
)

cc_library_mozc(
    name = "session_handler_test_util",
    testonly = 1,
    srcs = [
        "common.h",
        "session_handler.h",
        "session_handler_test_util.cc",
    ],
    hdrs = ["session_handler_test_util.h"],
    deps = [
        ":session",
        ":session_handler",
        ":session_handler_interface",
        ":session_usage_observer",
        "//base",
        "//base:config_file_stream",
        "//base:file_util",
        "//base:logging",
        "//base:port",
        "//base:system_util",
        "//composer:table",
        "//config:character_form_manager",
        "//config:config_handler",
        "//converter:converter_interface",
        "//engine:engine_builder_interface",
        "//engine:engine_interface",
        "//prediction:user_history_predictor",
        "//protocol:commands_proto",
        "//protocol:config_proto",
        "//storage:lru_cache",
        "//storage:registry",
        "//testing",
        "//testing:gunit_prod",
        "//usage_stats:usage_stats_testing_util",
        "@com_google_absl//absl/flags:flag",
    ],
)

cc_test_mozc(
    name = "session_handler_test",
    size = "large",
    srcs = [
        "common.h",
        "session_handler_test.cc",
    ],
    requires_full_emulation = False,
    deps = [
        ":session_handler",
        ":session_handler_test_util",
        "//base",
        "//base:clock_mock",
        "//base:port",
        "//base:stopwatch",
        "//base:util",
        "//config:config_handler",
        "//converter:converter_mock",
        "//engine:engine_builder_interface",
        "//engine:engine_stub",
        "//engine:mock_converter_engine",
        "//engine:mock_data_engine_factory",
        "//engine:user_data_manager_mock",
        "//protocol:commands_proto",
        "//protocol:config_proto",
        "//testing:gunit_main",
        "//usage_stats",
        "//usage_stats:usage_stats_testing_util",
        "@com_google_absl//absl/flags:flag",
        "@com_google_absl//absl/memory",
    ],
)

cc_library_mozc(
    name = "session_server",
    srcs = [
        "common.h",
        "session_server.cc",
    ],
    hdrs = ["session_server.h"],
    deps = [
        ":session_handler",
        ":session_usage_observer",
        "//base",
        "//base:logging",
        "//base:port",
        "//base:scheduler",
        "//engine:engine_factory",
        "//ipc",
        "//ipc:named_event",
        "//protocol:commands_proto",
        "//usage_stats:usage_stats_uploader",
    ],
)

cc_test_mozc(
    name = "session_server_test",
    size = "small",
    srcs = ["session_server_test.cc"],
    requires_full_emulation = False,
    tags = ["no_android"],
    deps = [
        ":session_server",
        "//base",
        "//base:scheduler",
        "//base:system_util",
        "//testing:gunit_main",
        "@com_google_absl//absl/flags:flag",
    ],
)

cc_library_mozc(
    name = "request_test_util",
    srcs = ["request_test_util.cc"],
    hdrs = ["request_test_util.h"],
    deps = [
        "//base",
        "//base:port",
        "//composer:table",
        "//protocol:commands_proto",
    ],
    # TODO(team): Ideally this target is testonly but evaluation/ shares
    # this target to change the request object in a scoped way. We can
    # again make this testonly after removing the global request object.
    # testonly = 1
)

cc_test_mozc(
    name = "request_test_util_test",
    size = "small",
    srcs = ["request_test_util_test.cc"],
    requires_full_emulation = False,
    deps = [
        ":request_test_util",
        "//protocol:commands_proto",
        "//testing:gunit_main",
    ],
)

cc_binary_mozc(
    name = "session_client_main",
    srcs = [
        "session_client_main.cc",
        "session_interface.h",
    ],
    copts = ["$(STACK_FRAME_UNLIMITED)"],  # session_client_main.cc
    deps = [
        ":session",
        "//base",
        "//base:file_stream",
        "//base:file_util",
        "//base:init_mozc",
        "//base:logging",
        "//base:port",
        "//base:system_util",
        "//base:util",
        "//composer:key_parser",
        "//engine:engine_factory",
        "//engine:engine_interface",
        "//protocol:commands_proto",
        "//protocol:config_proto",
        "@com_google_absl//absl/flags:flag",
        "@com_google_absl//absl/memory",
    ],
)

cc_binary_mozc(
    name = "session_server_main",
    srcs = ["session_server_main.cc"],
    copts = ["$(STACK_FRAME_UNLIMITED)"],  # session_server_main.cc
    deps = [
        ":session_server",
        "//base:init_mozc",
        "//protocol:commands_proto",
        "@com_google_absl//absl/flags:flag",
        "@com_google_absl//absl/strings:str_format",
    ],
)

cc_library_mozc(
    name = "session_watch_dog",
    srcs = ["session_watch_dog.cc"],
    hdrs = ["session_watch_dog.h"],
    deps = [
        "//base",
        "//base:clock",
        "//base:cpu_stats",
        "//base:logging",
        "//base:port",
        "//base:system_util",
        "//base:thread",
        "//base:unnamed_event",
        "//client",
        "//client:client_interface",
        "@com_google_absl//absl/memory",
    ],
)

cc_test_mozc(
    name = "session_watch_dog_test",
    size = "large",
    srcs = ["session_watch_dog_test.cc"],
    requires_full_emulation = False,
    tags = ["no_android"],
    deps = [
        ":session_watch_dog",
        "//base",
        "//base:cpu_stats",
        "//base:logging",
        "//base:mutex",
        "//base:port",
        "//base:util",
        "//client:client_mock",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "session_usage_observer",
    srcs = ["session_usage_observer.cc"],
    hdrs = ["session_usage_observer.h"],
    deps = [
        ":session_observer_interface",
        "//base",
        "//base:clock",
        "//base:logging",
        "//base:mutex",
        "//base:number_util",
        "//base:port",
        "//base:scheduler",
        "//config:stats_config_util",
        "//protocol:commands_proto",
        "//protocol:state_proto",
        "//usage_stats",
        "//usage_stats:usage_stats_proto",
    ],
)

cc_test_mozc(
    name = "session_usage_observer_test",
    size = "small",
    srcs = ["session_usage_observer_test.cc"],
    requires_full_emulation = False,
    deps = [
        ":session_usage_observer",
        "//base",
        "//base:clock_mock",
        "//base:logging",
        "//base:scheduler",
        "//base:scheduler_stub",
        "//base:system_util",
        "//base:util",
        "//config:stats_config_util",
        "//config:stats_config_util_mock",
        "//protocol:commands_proto",
        "//testing:gunit_main",
        "//usage_stats",
        "//usage_stats:usage_stats_proto",
        "//usage_stats:usage_stats_testing_util",
        "@com_google_absl//absl/flags:flag",
        "@com_google_absl//absl/memory",
    ],
)

cc_library_mozc(
    name = "ime_switch_util",
    srcs = ["ime_switch_util.cc"],
    hdrs = ["ime_switch_util.h"],
    deps = [
        ":key_info_util",
        "//base:port",
        "//base:singleton",
        "//config:config_handler",
        "//protocol:config_proto",
    ],
)

cc_test_mozc(
    name = "ime_switch_util_test",
    size = "small",
    srcs = ["ime_switch_util_test.cc"],
    requires_full_emulation = False,
    deps = [
        ":ime_switch_util",
        "//base",
        "//base:system_util",
        "//composer:key_parser",
        "//config:config_handler",
        "//protocol:commands_proto",
        "//protocol:config_proto",
        "//session/internal:keymap",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "key_info_util",
    srcs = ["key_info_util.cc"],
    hdrs = ["key_info_util.h"],
    deps = [
        "//base",
        "//base:config_file_stream",
        "//base:logging",
        "//base:port",
        "//base:util",
        "//composer:key_event_util",
        "//composer:key_parser",
        "//config:config_handler",
        "//protocol:commands_proto",
        "//protocol:config_proto",
        "//session/internal:keymap",
    ],
)

cc_test_mozc(
    name = "key_info_util_test",
    size = "small",
    srcs = ["key_info_util_test.cc"],
    requires_full_emulation = False,
    deps = [
        ":key_info_util",
        "//base",
        "//composer:key_parser",
        "//config:config_handler",
        "//protocol:commands_proto",
        "//protocol:config_proto",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "output_util",
    srcs = ["output_util.cc"],
    hdrs = ["output_util.h"],
    deps = [
        "//base",
        "//base:logging",
        "//base:port",
        "//protocol:commands_proto",
    ],
)

cc_test_mozc(
    name = "output_util_test",
    size = "small",
    srcs = ["output_util_test.cc"],
    requires_full_emulation = False,
    deps = [
        ":output_util",
        "//base",
        "//base:port",
        "//protocol:commands_proto",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "session_usage_stats_util",
    srcs = ["session_usage_stats_util.cc"],
    hdrs = ["session_usage_stats_util.h"],
    deps = [
        "//base",
        "//base:logging",
        "//base:port",
        "//base:util",
        "//base/protobuf",
        "//base/protobuf:descriptor",
        "//base/protobuf:message",
        "//protocol:commands_proto",
        "//usage_stats",
    ],
)

cc_test_mozc(
    name = "session_usage_stats_util_test",
    size = "small",
    srcs = ["session_usage_stats_util_test.cc"],
    requires_full_emulation = False,
    deps = [
        ":session_usage_stats_util",
        "//base",
        "//composer:key_parser",
        "//protocol:commands_proto",
        "//testing:gunit_main",
        "//usage_stats",
        "//usage_stats:usage_stats_testing_util",
    ],
)

cc_library_mozc(
    name = "session_handler_tool",
    srcs = ["session_handler_tool.cc"],
    hdrs = ["session_handler_tool.h"],
    deps = [
        ":request_test_util",
        ":session_handler",
        ":session_handler_interface",
        ":session_usage_observer",
        "//base:config_file_stream",
        "//base:file_stream",
        "//base:file_util",
        "//base:number_util",
        "//base:status",
        "//base:util",
        "//base/protobuf:descriptor",
        "//base/protobuf:message",
        "//base/protobuf:text_format",
        "//composer:key_parser",
        "//config:character_form_manager",
        "//config:config_handler",
        "//converter:converter_interface",
        "//engine:engine_factory",
        "//engine:engine_interface",
        "//engine:user_data_manager_interface",
        "//prediction:user_history_predictor",
        "//protocol:commands_proto",
        "//protocol:config_proto",
        "//storage:registry",
        "//usage_stats",
        "@com_google_absl//absl/memory",
        "@com_google_absl//absl/strings",
    ],
)

cc_binary_mozc(
    name = "session_handler_main",
    srcs = ["session_handler_main.cc"],
    deps = [
        ":session_handler_tool",
        "//base:file_stream",
        "//base:init_mozc",
        "//base:status",
        "//base:system_util",
        "//data_manager/android:android_data_manager",
        "//data_manager/google:google_data_manager",
        "//data_manager/oss:oss_data_manager",
        "//engine",
        "//protocol:commands_proto",
        "@com_google_absl//absl/flags:flag",
        "@com_google_absl//absl/strings",
    ],
)

cc_test_mozc(
    name = "session_handler_scenario_test",
    size = "medium",
    srcs = ["session_handler_scenario_test.cc"],
    data = [
        "//data/test/session/scenario:scenario_files",
        "//data/test/session/scenario/usage_stats:usage_stats_scenario_files",
    ],
    requires_full_emulation = False,
    deps = [
        ":session_handler",
        ":session_handler_test_util",
        ":session_handler_tool",
        "//base:config_file_stream",
        "//base:file_stream",
        "//base:file_util",
        "//base:number_util",
        "//base:status",
        "//base:util",
        "//base/protobuf:descriptor",
        "//base/protobuf:message",
        "//base/protobuf:text_format",
        "//converter:converter_interface",
        "//engine:mock_data_engine_factory",
        "//prediction:user_history_predictor",
        "//protocol:commands_proto",
        "//protocol:config_proto",
        "//storage:registry",
        "//testing:gunit_main",
        "//testing:mozctest",
        "//usage_stats",
        "//usage_stats:usage_stats_testing_util",
        "@com_google_absl//absl/memory",
        "@com_google_absl//absl/strings",
    ],
)

cc_library_mozc(
    name = "session_handler_interface",
    hdrs = ["session_handler_interface.h"],
    visibility = [
        "//:__subpackages__",
    ],
    deps = [
        "//base:port",
        "@com_google_absl//absl/strings",
    ],
)

cc_library_mozc(
    name = "session_converter_interface",
    hdrs = ["session_converter_interface.h"],
    visibility = ["//session/internal:__pkg__"],
    deps = [
        "//base:port",
        "//converter",
        "//converter:segments",
        "//protocol:config_proto",
        "//transliteration",
    ],
)

cc_library_mozc(
    name = "session_observer_interface",
    hdrs = ["session_observer_interface.h"],
    visibility = ["//visibility:private"],
    deps = ["//base:port"],
)

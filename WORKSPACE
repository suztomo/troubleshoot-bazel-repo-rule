workspace(
    name = "com_google_googleapis",
)   
    
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

# Protobuf depends on very old version of rules_jvm_external.
# Importing older version of rules_jvm_external first (this is one of the things that protobuf_deps() call
# below will do) breaks the Java client library generation process, so importing the proper version explicitly before calling protobuf_deps().
RULES_JVM_EXTERNAL_TAG = "4.5"

RULES_JVM_EXTERNAL_SHA = "b17d7388feb9bfa7f2fa09031b32707df529f26c91ab9e5d909eb1676badd9a6"
    
http_archive(
    name = "rules_jvm_external",
    sha256 = RULES_JVM_EXTERNAL_SHA,
    strip_prefix = "rules_jvm_external-%s" % RULES_JVM_EXTERNAL_TAG,
    url = "https://github.com/bazelbuild/rules_jvm_external/archive/%s.zip" % RULES_JVM_EXTERNAL_TAG,
)

load("@rules_jvm_external//:repositories.bzl", "rules_jvm_external_deps")
rules_jvm_external_deps()
load("@rules_jvm_external//:setup.bzl", "rules_jvm_external_setup")
rules_jvm_external_setup()


load("//:lts_config.bzl", "lts_rules")

lts_rules(name = "java_lts_rules")
    
load("@java_lts_rules//:lts_config.bzl", "GAPIC_GENERATOR_JAVA_VERSION", "LTS")

print("LTS = {}".format(LTS))

_gapic_generator_java_version = GAPIC_GENERATOR_JAVA_VERSION
# _gapic_generator_java_version = "2.18.0"

print("_gapic_generator_java_version = {}".format(_gapic_generator_java_version))

load("@rules_jvm_external//:defs.bzl", "maven_install")
maven_install(
    artifacts = [
        "com.google.api:gapic-generator-java:" + _gapic_generator_java_version,
    ],
    #Update this False for local development
    fail_on_missing_checksum = True,
    repositories = [
        "m2Local",
        "https://repo.maven.apache.org/maven2/",
    ],
) 

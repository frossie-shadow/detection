# -*- python -*-
#
# Setup our environment
#
import glob, os.path, re, os
import lsst.SConsUtils as scons

env = scons.makeEnv("detection",
                    r"$HeadURL$",
                    [["boost", "boost/version.hpp", "boost_system:C++"],
                     ["boost", "boost/version.hpp", "boost_filesystem:C++"],
                     ["boost", "boost/regex.hpp", "boost_regex:C++"],
                     ["python", "Python.h"],
		     ["m", "math.h", "m", "sqrt"],
		     ["cfitsio", "fitsio.h", "cfitsio", "ffopen"],
                     ["wcslib", "wcslib/wcs.h", "wcs"],
                     ["xpa", "xpa.h", "xpa", "XPAPuts"],
                     ["utils", "lsst/utils/Utils.h", "utils:C++"],
                     ["daf_base", "lsst/daf/base/DataProperty.h", "daf_base:C++"],
                     ["pex_policy", "lsst/pex/policy/Policy.h", "pex_policy:C++"],
                     ["pex_exceptions", "lsst/pex/exceptions.h", "pex_exceptions:C++"],
                     ["pex_logging", "lsst/pex/logging/Trace.h", "pex_logging:C++"],
                     ["security", "lsst/security/Security.h", "security:C++"],
                     ["daf_persistence", "lsst/daf/persistence/Persistence.h", "daf_persistence:C++"],
                     ["daf_data", "lsst/daf/data/LsstBase.h", "daf_data:C++"],
                     ["afw", "lsst/afw/image/MaskedImage.h", "afw"]
                     ])

env.libs["detection"] +=  env.getlibs("daf_base daf_data daf_persistence pex_logging pex_exceptions pex_policy security afw boost utils wcslib")
#
# Build/install things
#
for d in Split("doc include/lsst/%s lib src tests" % env['eups_product']):
    SConscript(os.path.join(d, "SConscript"))

for d in map(lambda str: os.path.join("python/lsst/%s" % env['eups_product'], str), Split(".")):
    SConscript(os.path.join(d, "SConscript"))

env['IgnoreFiles'] = r"(~$|\.pyc$|^\.svn$|\.o$)"

Alias("install", env.Install(env['prefix'], "python"))
Alias("install", env.Install(env['prefix'], "include"))
Alias("install", env.Install(env['prefix'], "lib"))
Alias("install", env.Install(env['prefix'], "pipeline"))
Alias("install", env.Install(env['prefix'] + "/bin", glob.glob("bin/*.py")))
Alias("install", env.InstallEups(env['prefix'] + "/ups", glob.glob("ups/*.table")))

scons.CleanTree(r"*~ core *.so *.os *.o")
#
# Build TAGS files
#
files = scons.filesToTag()
if files:
    env.Command("TAGS", files, "etags -o $TARGET $SOURCES")

env.Declare()
env.Help("""
LSST FrameWork packages
""")


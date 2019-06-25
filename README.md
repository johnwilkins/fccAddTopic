# fccAddModule

The fccAddModule.py program is a helper utility for adding a module in Red Hat's
Flexible Customer Content format (FCC). The primary purpose for the program is
to enable writers to specify a module name in plain text and an assembly file name. The program will:

* Generate the module file
* Add an anchor tag to the module
* Add a heading with the specified module name
* Generate the module file name
* Save the module file; and,
* Add/append an include statement to the assembly, if specified.


To use the program, copy fccAddModule.conf and fccAddModule.py to your project.
Modify the default values in fccAddModule.conf as needed. Generally, you should modify the component type and destination path. The program must be run in the same directory as `master.adoc` and the assembly files. For example:

----
$ python fccAddModule.py --name "My Module Name" --assembly assembly_assembly-file-name_en-us.adoc
----

The foregoing usage generates a file called `proc_comp_my-module-name_en-us.adoc` where `proc` is the default module type of procedure, `comp` is the component name, `my-module-name` is the name of the module, and `en_us` specifies the locale.

The generated file contents include:

----
[id='my-module-name-{context}']

= My Module Name
----

The program appends an include statement to the assembly file, if specified:
----
include::{includedir}/<moduleDestinationPath>/proc_comp_my-module-name_en-us.adoc[leveloffset=+1]
----

The program assumes the use of an `{includedir}` variable. The `moduleDestinationPath` is the location where the program stores modules. With modular forma

Options include:

-n '<moduleName>' OR --name '<moduleName>' (REQUIRED)
-t (proc|con|ref) OR --type (proc|con|ref) OPTIONAL. DEFAULT = proc
-a <assemblyFile> OR --assembly <assemblyFile> (OPTIONAL)
-c <componentName> OR --component <componentName> May set default in fccAddModule.conf
-s <sourceFileName> OR --source <sourceFileName> The source file to include. OPTIONAL

The `-t` or `--type` option specifies the type of module. The options are `proc` for procedure, `con` for concept, and `ref` for reference. By default, the program uses `proc`. You may override the default with the `-t` or `--type` option on the command line, or set a new default in the `fccAddModule.conf` configuration file. Procedure is the most common module type so it is the default value.

The component name should be the product name or a sub-component of the product. For example, "ceph" as a product or "rbd", "rgw", "rados", or `cephfs` as components of Ceph Storage; "osp" as a product, or "heat", "nova", "glance", etc. as components of OpenStack Platform. When working on a project, modify the `defaultComponentType` setting of the `fccAddModule.conf` file.
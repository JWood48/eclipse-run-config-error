# eclipse-run-config-error

A gradle project with a **generated** SourceSet will not have the compiled classes from the generated source set available when starting the application in eclipse!

Preconditions:

Eclipse: 2018-12
Buildship: 3.0.0

Reproduce:

1. run gradle generate
1. run gradle run - should be no problems
1. import project in eclipse
1. Right click eclipse.run.config.error.App.java -> Run as -> Java Application
  * should fail with **java.lang.NoClassDefFoundError: eclipse/run/config/error/GeneratedClass**


TO make it work comment in the following in the build.gradle file
```
eclipse {
	classpath {
		file {
			whenMerged {
				entries.findAll { (it.properties.kind.equals('src') || it.properties.kind.equals('lib')) && it.entryAttributes.get('gradle_scope').equals('generated') }.each {
					it.entryAttributes['gradle_used_by_scope'] = 'generated,main'
				}
			}
		}
	}
}
```

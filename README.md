## Micronaut 2.5.1 ECJ Package Name Bug

### micronaut 2.5.0
Running `./gradlew ecj` with `micronautVersion=2.5.0` generates:
```
build/ecj/classes/micronaut/bug/$Application$InnerSingletonDefinition.class
build/ecj/classes/micronaut/bug/$Application$InnerSingletonDefinitionClass.class
build/ecj/classes/micronaut/bug/Application$InnerSingleton.class
build/ecj/classes/micronaut/bug/Application.class
build/ecj/classes/META-INF/services/io.micronaut.inject.BeanDefinitionReference
```

### micronaut 2.5.1
Running `./gradlew ecj` with `micronautVersion=2.5.1` generates:
```
build/ecj/classes/package micronaut/bug/$Application$InnerSingletonDefinition.class
build/ecj/classes/package micronaut/bug/$Application$InnerSingletonDefinitionClass.class
build/ecj/classes/META-INF/services/io.micronaut.inject.BeanDefinitionReference
build/ecj/classes/micronaut/bug/Application.class
build/ecj/classes/micronaut/bug/Application$InnerSingleton.class
```

### micronaut core commit 356945845
micronaut-core/inject/src/main/java/io/micronaut/inject/processing/JavaModelUtils.java:
```
    public static String getPackageName(TypeElement typeElement) {
        Element enclosingElement = typeElement.getEnclosingElement();
        while (enclosingElement != null && enclosingElement.getKind() != ElementKind.PACKAGE) {
            enclosingElement = enclosingElement.getEnclosingElement();
        }
        if (enclosingElement == null) {
            return StringUtils.EMPTY_STRING;
        } else {
            return enclosingElement.toString();
        }
    }
```

The toString() method of element on ecj returns 'package name' rather than 'name'.

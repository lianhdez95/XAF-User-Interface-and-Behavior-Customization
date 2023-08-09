# Interfaz de usuario y personalización del comportamiento

La construcción de la interfaz de usuario es un paso principal en la creación de una aplicación empresarial. eXpressApp Framework genera automáticamente una interfaz de usuario basada en su modelo de negocio y el modelo de aplicación. Puede usar la interfaz de usuario predeterminada o personalizarla a través de un modelo de aplicación o en código. Los temas de esta sección proporcionan información detallada sobre los elementos de la interfaz de usuario de XAF y los mecanismos de su creación e interacción.


# Modelo de aplicación (almacenamiento de configuración de interfaz de usuario)

eXpressApp Framework utiliza la misma lógica de negocio de la aplicación para construir la interfaz de usuario para diferentes plataformas de destino. El marco identifica las clases de modelo de negocio, las analiza y crea metadatos: un archivo XML que describe una aplicación. Esos metadatos son el  **modelo de aplicación**. XAF lo usa para crear la interfaz de usuario de la aplicación para la plataforma requerida.

Por ejemplo, puede declarar la siguiente clase simple:

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/56a456df-5756-41b0-b7cc-3dcf1d9d3c43)


XAF identifica esta clase como parte del modelo de negocio y genera un nodo correspondiente en el modelo de  **aplicación**. Este es el aspecto del nodo en el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor)

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/ae60e310-9d23-41d9-b146-17e0dc487053)


Basándose en este  **modelo de aplicación**, XAF genera una interfaz de usuario para las plataformas Blazor, WinForms y ASP.NET WebForms. La siguiente imagen muestra un ejemplo de Blazor.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/385bd60d-036f-4497-8a73-6e97078f9d6e)


# Cómo funciona el modelo de aplicación XAF


El  **modelo de aplicación**  son metadatos que definen  **la estructura de navegación, los formatos de presentación de datos y los comandos disponibles**. XAF crea estos metadatos basándose en el código de la aplicación. Puede ampliar o personalizar el modelo de aplicación para agregar esta funcionalidad a la aplicación. Los usuarios también pueden acceder al  **modelo de aplicación**  para que puedan ajustar sus aplicaciones.

La siguiente imagen muestra el cuadro de diálogo integrado  **Editor de modelos**. Pruebe el comando  **Editar modelo**  en cualquiera de las demostraciones de  **eXpressApp Framework**  para acceder a este editor.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/393d0bea-f4aa-4b23-b124-d4e334d3f24a)


## Cómo XAF utiliza el modelo de aplicación

Al ejecutar una aplicación XAF, el marco realiza los pasos siguientes:

1.  Examina el código de la aplicación para generar el modelo de  **aplicación**.
2.  Rellena el  **modelo de aplicación**  con valores predeterminados. Obtenga información sobre  [los generadores de nodos](https://docs.devexpress.com/eXpressAppFramework/113316/ui-construction/application-model-ui-settings-storage/how-application-model-works/built-in-nodes-generators)  y  [los extensores de modelos](https://docs.devexpress.com/eXpressAppFramework/403535/ui-construction/application-model-ui-settings-storage/how-application-model-works/application-model-interfaces)  para comprender cómo funciona este proceso y cómo puede controlarlo. Para obtener ejemplos de extensión, vea  [Extender y personalizar el modelo de aplicación en código](https://docs.devexpress.com/eXpressAppFramework/112810/ui-construction/application-model-ui-settings-storage/customize-application-model-in-code/access-the-application-model-in-code).
3.  Busca diferencias de modelo guardadas anteriormente que cambian los valores predeterminados. (Estas podrían ser ediciones realizadas por el usuario de ejecuciones de aplicaciones anteriores). Aplica estos cambios.
4.  Crea elementos de interfaz de usuario basados en información actualizada del  **modelo de aplicación**.
5.  Puede cambiar el  **modelo de aplicación**  después de que XAF cree la interfaz de usuario de la aplicación. En tales casos, debe forzar una actualización de la interfaz de usuario para reflejar estos cambios. Para obtener una solución alternativa, vea el artículo siguiente:  [Aplicar cambios en el modelo de aplicación a la vista actual inmediatamente](https://docs.devexpress.com/eXpressAppFramework/118592/ui-construction/application-model-ui-settings-storage/customize-application-model-in-code/apply-application-model-changes-to-the-current-view-immediately).

## Qué código participa en la generación del modelo de aplicación

XAF llama a los métodos  [GetExportedTypes()/GetControllerTypes()](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ModuleBase.GetExportedTypes)  de cada  [módulo](https://docs.devexpress.com/eXpressAppFramework/118046/application-shell-and-base-infrastructure/application-solution-components/modules)  para descubrir las clases que forman parte de la estructura del  **modelo de aplicación**.[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ModuleBase.GetControllerTypes)  Estos métodos utilizan la  [reflexión](https://docs.microsoft.com/en-us/dotnet/framework/reflection-and-codedom/reflection) para localizar las clases enumeradas a continuación.

-   Modelo  
    de  **objetos de negocio**  Estas clases constituyen el nodo  **BOModel**. Para obtener más información sobre qué clases llegan a este nodo, vea  [Agregar una clase empresarial](https://docs.devexpress.com/eXpressAppFramework/112847/business-model-design-orm/ways-to-add-a-business-class)  y  [objetos no persistentes](https://docs.devexpress.com/eXpressAppFramework/116516/business-model-design-orm/non-persistent-objects).
-   **Controlador y acciones**  
    Estas clases constituyen el nodo  **ActionDesign**. Para obtener información adicional, consulte  [Controladores y acciones](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers).

>NOTA
XAF utiliza sólo clases públicas para crear el **modelo de aplicación**.

## API de modelo de aplicación

El  **modelo de aplicación**  es un árbol de nodos. Cada tipo de nodo expone su propio conjunto de propiedades.

### Tipos de nodos / Interfaces

XAF define una interfaz para cada tipo de nodo del  **modelo de aplicación**. Todas estas interfaces comparten un ancestro común:  [IModelNode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelNode). Los miembros de esta interfaz base permiten la modificación de propiedades y el acceso a los nodos padre y secundario.

Uno de los descendientes es la interfaz  [IModelApplication](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelApplication)  que corresponde al nodo raíz (**Application**). Esta interfaz define algunas propiedades de nivel de aplicación (**Título**,  **Empresa**,  **Descripción**) y nodos secundarios (**ActionDesign**,  **Vistas**  y otros).

Revise todas las interfaces de nodo disponibles:  [Modelo de aplicación - Interfaces integradas](https://docs.devexpress.com/eXpressAppFramework/403535/ui-construction/application-model-ui-settings-storage/how-application-model-works/application-model-interfaces).

### Extensores de nodos

XAF le permite ampliar el modelo de aplicación: agregar propiedades o nodos secundarios a los nodos existentes. La idea básica es que una interfaz "extensora" (la que tiene propiedades adicionales o hijos) puede conectarse a un nodo.

Puede utilizar extensores  [integrados, como IModelListViewFilters](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelListViewFilters). Esta interfaz extiende un nodo  **ListView**  con el elemento secundario  **Filters**. Para obtener la lista completa de interfaces de extensor  [integradas, consulte Modelo de aplicación - Interfaces integradas](https://docs.devexpress.com/eXpressAppFramework/403535/ui-construction/application-model-ui-settings-storage/how-application-model-works/application-model-interfaces).

También puede extender los nodos del  **modelo de aplicación**  con propiedades y elementos secundarios personalizados. Para obtener ejemplos, consulte el artículo siguiente:  [Cómo extender el modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112810/ui-construction/application-model-ui-settings-storage/customize-application-model-in-code/access-the-application-model-in-code).

### Obtener acceso y modificar el modelo de aplicación en código

Puede escribir código que obtenga el objeto Application Model, obtenga acceso a nodos secundarios específicos y cambie su configuración. Para obtener ejemplos, consulte el artículo siguiente:  [Cambiar el modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/403527/ui-construction/application-model-ui-settings-storage/change-application-model).

## Capas del modelo de aplicación

Por un lado, XAF promueve la reutilización de código y el mismo modelo de  **negocio**  y  **controladores**  pueden servir en múltiples aplicaciones para diferentes plataformas.

Por otro lado, diferentes aplicaciones pueden beneficiarse de las variaciones en el diseño de la interfaz de usuario. Diferentes usuarios también pueden adaptar las aplicaciones a sus propios requisitos.

Esto significa que XAF debe mantener el modelo de aplicación básico y las variaciones que se aplican al modelo en diferentes niveles:

-   **El**  XAF de capa cero genera esta capa  
    basándose en el código de los módulos de aplicación y los módulos a los que se hace referencia (como se describió anteriormente en este artículo).
-   **Diferencias**  
    a nivel de módulo Estas diferencias específicas de la plataforma existen para cada módulo de aplicación.
-   **Diferencias**  
    de administrador Estas diferencias específicas del proyecto existen para cada proyecto de aplicación.
-   **Diferencias**  
    de usuario Personalizaciones del usuario final al modelo de aplicación. Estas diferencias existen en cada máquina de usuario final.

Si diferentes capas tienen valores diferentes para la misma propiedad, XAF utiliza el valor de un nivel superior. Por ejemplo, un valor de la capa 'Diferencias de usuario' tiene una prioridad más alta que la de la capa 'Nivel de módulo'.

La siguiente imagen ilustra esta estructura en capas:

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/101c916f-7e8b-4e8b-8f8f-58eafad43065)


## Diferencias en el modelo de aplicación: tipos de almacenamiento

XAF puede almacenar cambios en el modelo de aplicación (diferencias de modelo) en diferentes medios. La clase implementa la funcionalidad básica de almacenamiento. Están disponibles los siguientes descendientes: `ModelStoreBase`

-   `FileModelStore` - Archivo XAFML
-   `ResourcesModelStore` - Recurso del proyecto
-   `ModelDifferenceDbStore` -Base de datos
-   `CookiesModelDifferenceStore` - Cookies (ASP.NET formularios web)

Los tipos de almacenamiento predeterminados para las capas se enumeran en la tabla siguiente:

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/63f84b14-1b5e-4777-abd9-539fa13becd5)


Puede cambiar el tipo de almacenamiento para las diferencias de administrador y usuario. Suscríbase a los siguientes eventos:

-   Diferencias de administrador:  [CreateCustomModelDifferenceStore](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.XafApplication.CreateCustomModelDifferenceStore)
-   Diferencias de usuario:  [CreateCustomUserModelDifferenceStore](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.XafApplication.CreateCustomUserModelDifferenceStore)

### Detalles del almacenamiento de archivos

Para almacenar las diferencias de modelo en archivos, XAF utiliza archivos *.xafml que pueden ser editados por el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor). Hay dos tipos de archivos *.xafml.

-   **Diferencias de modelo**: contiene personalizaciones generales de la interfaz de usuario
-   **Aspectos de diferencia del modelo**: contiene personalizaciones de interfaz de usuario localizadas

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/06d4d467-dfd7-4db6-89db-5a874f408cca)


>PROPINA
Para obtener más información sobre la localización de modelos de aplicación, consulte el siguiente artículo: [Conceptos básicos de la localización](https://docs.devexpress.com/eXpressAppFramework/112595/localization/localization-basics).

En la tabla siguiente se enumeran los nombres y las ubicaciones de los archivos *.xafml para diferentes capas del modelo de aplicación.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/2db8724e-6db3-47ca-bdb4-48403016878b)


La ubicación Diferencias del modelo de usuario se especifica en el archivo  _App.config._  Vea el código a continuación.

(.xml)
```xml
<appSettings>
    <!-- ... -->
    <add key="UserModelDiffsLocation" value="CurrentUserApplicationDataFolder"/>
    <!-- ... -->
</appSettings>

```

Los valores posibles son:

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/102a810b-a934-442d-be2d-31c8f173a28e)


### Detalles del almacenamiento de la base de datos

>PROPINA
Si necesita habilitar el  **almacenamiento**  de base de datos en  una aplicación existente, consulte el siguiente tema de ayuda: [Almacenar diferencias del modelo de aplicación en la base de datos](https://docs.devexpress.com/eXpressAppFramework/113698/ui-construction/application-model-ui-settings-storage/application-model-storages/store-the-application-model-differences-in-the-database).

XAF utiliza dos entidades para almacenar las diferencias de modelo en la base de datos. Las entidades forman una relación de uno a muchos.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/8797e761-029b-4b98-8d26-29f64366861c)


#### Diferencia de modelo

La interfaz  [IModelDifference](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.IModelDifference)  almacena una colección de  **aspectos de diferencia de modelo**  para un usuario.

Cada  **diferencia de modelo**  tiene un usuario asociado ([IModelDifference.UserId](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.IModelDifference.UserId)). Un  **UserId**  vacío significa que la diferencia de modelo actual es compartida por todos los usuarios y se superpone con su configuración individual.

Una diferencia de modelo también define un  [IModelDifference.ContextId](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.IModelDifference.ContextId)  que corresponde a la plataforma de destino. De esta manera, se pueden usar conjuntos separados de diferencias para el mismo usuario cuando ejecuta aplicaciones en un escritorio o en la web.

#### Aspecto de diferencia de modelo

La interfaz IModelDifferenceAspect almacena una configuración modificada en la propiedad  [IModelDifferenceAspect.Xml.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.IModelDifferenceAspect.Xml)[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.IModelDifferenceAspect)

La propiedad  [IModelDifferenceAspect.Name](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.IModelDifferenceAspect.Name)  establece la configuración regional para la diferencia. Si el valor de la propiedad está vacío, el aspecto se aplica a todos los idiomas.

>IMPORTANTE
Si utiliza el modo de seguridad integrado o el servidor de nivel intermedio, asegúrese de que todos los usuarios tengan acceso de lectura y escritura a los tipos de aspecto Diferencia de modelo  y **Diferenciade modelo** (los usuarios con permisos de sólo lectura no pueden conservar las personalizaciones de la aplicación al salir). Se requieren permisos  **de lectura y escritura** para el _modelo.Operación_ de importación de archivos XAFML. En el escenario de nivel intermedio, se requiere adicionalmente el permiso  **Crear**.

>NOTA
Como alternativa para otorgar a un usuario permisos de solo lectura, puede usar la [aplicación Win.Omitir las diferencias del modelo de usuario para omitirlas diferencias del modelodel usuario](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.WinApplication.IgnoreUserModelDiffs).

#### Gestión de diferencias de modelos compartidos

El  [Asistente para soluciones](https://docs.devexpress.com/eXpressAppFramework/113624/installation-upgrade-version-history/visual-studio-integration/solution-wizard)  genera, pero comenta el código que habilita el almacenamiento de bases de datos para las diferencias de modelo compartido (configuración del administrador). Ese código está disponible en los archivos WinModule.cs (_WinModule_.vb) y WebModule_.cs_  (_WebModule.vb_).

El comportamiento predeterminado de XAF es cargar siempre la configuración en tiempo de diseño.

Puede quitar la marca de comentario de la suscripción al evento  [XafApplication.CreateCustomModelDifferenceStore.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.XafApplication.CreateCustomModelDifferenceStore)  El contenido del archivo  _Model.xafml_  se cargará en la base de datos una vez iniciada la aplicación. XAF omitirá los cambios adicionales en este archivo si el registro de base de datos ya existe para las diferencias de modelo compartido. Para volver a cargar la configuración desde  _Model.xafml_,  [habilite la interfaz de usuario administrativa](https://docs.devexpress.com/eXpressAppFramework/113704/ui-construction/application-model-ui-settings-storage/application-model-storages/enable-the-administrative-ui-for-managing-users-model-differences)  y use la acción  **Importar diferencia**  de modelo compartido (o elimine el registro Diferencia de modelo compartido y reinicie).

>NOTA
La siguiente combinación de características no es compatible cuando se usan juntas.
>- [SecuredObjectSpaceProvider](https://docs.devexpress.com/eXpressAppFramework/113437/data-security-and-safety/security-system/security-tiers/2-tier-security-integrated-mode-and-ui-level/change-the-client-side-security-mode-from-ui-level-to-integrated-in-xpo-applications) o [XPObjectSpaceProvider](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Xpo.XPObjectSpaceProvider) creado con el constructor con el parámetro threadSafe establecido en verdadero (este parámetro habilita [ThreadSafeDataLayer](https://docs.devexpress.com/XPO/DevExpress.Xpo.ThreadSafeDataLayer)).
>-Las diferencias de modelo de toda la aplicación [se almacenan en la base de datos](https://docs.devexpress.com/eXpressAppFramework/113698/ui-construction/application-model-ui-settings-storage/application-model-storages/store-the-application-model-differences-in-the-database) mediante el evento [XafApplication.CreateCustomModelDifferenceStore](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.XafApplication.CreateCustomModelDifferenceStore) (todavía puede almacenar las diferencias específicas del usuario en la base de datos mediante el evento [XafApplication.CreateCustomUserModelDifferenceStore](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.XafApplication.CreateCustomUserModelDifferenceStore)).
>- [Campos persistentes personalizados](https://docs.devexpress.com/eXpressAppFramework/113583/business-model-design-orm/types-info-subsystem/use-metadata-to-customize-business-classes-dynamically) declarados en las diferencias del modelo a nivel de aplicación.
>
>En esta configuración, su aplicación carga información sobre campos persistentes personalizados de la base de datos y luego actualiza el esquema de la base de datos. Sin embargo, una capa de datos segura para subprocesos no admite la modificación del modelo de datos después de establecer la conexión con la base de datos.

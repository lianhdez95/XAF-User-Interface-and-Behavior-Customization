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


# Modelo de aplicación: interfaces integradas

En este tema se enumeran las interfaces y los extensores del  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112579/ui-construction/application-model-ui-settings-storage)  suministrados con XAF. XAF utiliza extensores para agregar propiedades a los nodos del modelo de aplicación.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/cf533e51-4e67-4d8e-9500-bc2a5fc7eb60)


# Modelo de aplicación: Generadores de nodos incorporados


Internamente, los módulos XAF utilizan los enfoques descritos en el tema  [Extender y personalizar el modelo de aplicación en código](https://docs.devexpress.com/eXpressAppFramework/112810/ui-construction/application-model-ui-settings-storage/customize-application-model-in-code/access-the-application-model-in-code)  para generar el contenido del modelo de aplicación. En este tema se proporciona una lista de generadores de nodos integrados. Puede utilizar esta lista al personalizar el modelo de aplicación mediante la implementación de un actualizador de generador. Aquí, puede averiguar qué generadores de nodos están disponibles para la personalización.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/b3c80b4c-3414-410e-95c0-0c573a5868b9)


## Node Generators in Extra Modules

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/bdbf3ff0-83e6-467c-b7b3-bbe7fa944e4b)

# Cambiar el modelo de aplicación

## Usar el Editor de modelos

El modelo de aplicación puede establecer configuraciones en varios niveles. Puede examinar y editar la configuración en cualquier nivel que utilice un archivo XAFML. Esto incluye todas las capas, excepto el nivel cero (configuración generada en función del código del modelo de negocio).

Para editar la configuración en cualquiera de esos niveles, utilice el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor).

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/4fa91fad-1239-42c2-8eed-9f2aa3eb7ec2)


## Obtener o establecer la configuración del modelo en el código

Puede acceder al modelo de aplicación en código y leer o modificar los valores necesarios.

>IMPORTANTE
Tenga en cuenta que la interfaz de usuario no refleja inmediatamente los cambios realizados en el modelo de aplicación. Si necesita aplicar cambios **después**  de  que XAF cree e inicialice un control de interfaz de usuario, acceda directamente al control. Consulte [Cómo: Obtener más información al componente de cuadrícula en una vista de lista](https://docs.devexpress.com/eXpressAppFramework/402154/ui-construction/list-editors/how-to-access-list-editor-control). También puede volver a crear un control con los cambios más recientes del modelo de aplicación, tal como se describe en el artículo siguiente: [Aplicar cambios del modelo de aplicación a la vista actual inmediatamente](https://docs.devexpress.com/eXpressAppFramework/118592/ui-construction/application-model-ui-settings-storage/customize-application-model-in-code/apply-application-model-changes-to-the-current-view-immediately).

Para tener acceso al modelo de aplicación en código, utilice los siguientes objetos:

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/ecf57b96-dcdb-40f5-9d33-8c9580fb1e8a)


These properties return an  [IModelNode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelNode)  descendant that encapsulates the corresponding node. You can use the  **Application**  property to access the Application Model’s root node. Refer to the following topic for additional information:  [How the XAF Application Model Works](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works).

The following code shows how to access the ‘Contact’ business class and modify its  [IModelClass.Caption](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelClass.Caption):


```csharp
using System.Linq;
using DevExpress.ExpressApp;
using DevExpress.ExpressApp.Actions;

namespace YourSolutionName.Module.Controllers {
    public class CustomController : ViewController {
        public CustomController() {
            var myAction1 = new SimpleAction(this, "MyAction1", null);
            myAction1.Execute += MyAction1_Execute;
        }

        private void MyAction1_Execute(object sender, 
         SimpleActionExecuteEventArgs e) {
            var lst = Application.Model.BOModel.ToList();
            var bo = Application.Model.BOModel.Where(x => x.Name == 
             "YourSolutionName.Module.BusinessObjects.Contact").FirstOrDefault();
            if(bo != null) {
                var oldCaption = bo.Caption;
                bo.Caption = "New test caption";
            }
        }
    }
}
```


# Editor de modelos: personalizar visualmente el modelo de aplicación


>IMPORTANTE
Para probar el Editor de modelos con proyectos de .NET 6+, asegúrese de que los siguientes componentes están instalados en el equipo:
>-   [.NET 6.  0 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/6.0)
>-   [Visual Studio 2022](https://visualstudio.microsoft.com/vs/)

Si ha compartido módulos XAF que deben reutilizarse en aplicaciones de .NET Framework y .NET 6+, puede establecer temporalmente como destino .NET Standard 2.  0 en estos proyectos compartidos, como se describe en el siguiente artículo: [Cómo migrar un módulo compartido XAF de .NET Framework a .NET Standard 2.0+](https://docs.devexpress.com/eXpressAppFramework/403370/overview/net-5-support-and-migration/port-xaf-shared-module).

El  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112579/ui-construction/application-model-ui-settings-storage)  contiene metadatos que definen la interfaz de usuario y el comportamiento de la aplicación. Esos metadatos se forman primero a partir del código del modelo de negocio. Diferentes módulos pueden aplicar sus propios cambios. Los usuarios finales también pueden modificar el  **modelo de aplicación**  para ajustar su entorno de trabajo.

Puede examinar y editar el  **modelo de aplicación**  en cualquiera de esas capas. Para ello, utilice el  **Editor de modelos**.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/34e4d416-7a39-4f82-adcb-445bb6e335b6)


# Formas de invocar el editor de modelos


El  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor)  se puede utilizar:

-   en  **tiempo de diseño (en**  Visual Studio por un desarrollador)
    
    El Editor de modelos invocado en tiempo de diseño es un panel de ventana en Visual Studio.
    
-   **En tiempo de ejecución**  (por usuarios y administradores de la aplicación)
    
    El Editor de modelos invocado por los usuarios y administradores es un formulario.
    

Los elementos y capacidades en ambos casos son principalmente los mismos.

En este tema se describe cómo invocar el Editor de modelos en diferentes escenarios.

## Invocar el Editor de modelos en Visual Studio en tiempo de diseño

En el Explorador de soluciones, haga doble clic en el archivo Model.xafml o  _Model.DesignedDiffs.xafml_  contenido en el proyecto de la solución XAF (módulo o proyecto de aplicación).

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/f0429112-490f-439f-a063-36d0f646b003)


Puede hacer clic con el botón derecho en el proyecto y elegir  **Abrir editor de modelos**  en el menú contextual.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/bf2d4340-6401-4dca-b1e5-ca0b5a6080be)


El Editor de modelos se invocará en un panel de ventana.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/2c719a27-d28f-46c6-b170-8649c917ddbf)


El panel del editor de modelos contiene los siguientes elementos:  [árbol de nodos](https://docs.devexpress.com/eXpressAppFramework/113328/ui-construction/application-model-ui-settings-storage/model-editor/node-tree),  [cuadrícula de propiedades](https://docs.devexpress.com/eXpressAppFramework/113329/ui-construction/application-model-ui-settings-storage/model-editor/property-grid)  y  [panel de búsqueda](https://docs.devexpress.com/eXpressAppFramework/113331/ui-construction/application-model-ui-settings-storage/model-editor/search-pane)  (opcional). Para cambiar el foco entre los elementos de la interfaz de usuario, presione la tecla TAB. En la parte inferior se muestra una descripción y un tipo de nodo/propiedad seleccionados.

El Editor de modelos muestra información diferente para diferentes proyectos. Esta información se recopila de los módulos que se agregan al proyecto para el que invocó el Editor de modelos. Si este proyecto es un módulo en sí, la información sobre él también se carga. Para los proyectos de .NET Framework, puede ver la lista de módulos cargados en el  [Diseñador de módulos](https://docs.devexpress.com/eXpressAppFramework/112828/installation-upgrade-version-history/visual-studio-integration/module-designer)  o en el  [Diseñador de aplicaciones](https://docs.devexpress.com/eXpressAppFramework/112827/installation-upgrade-version-history/visual-studio-integration/application-designer).

Todo lo que cambie en el Editor de modelos se guarda en el archivo Model.xafml o  _Model.DesignedDiffs.xafml_, dependiendo de si los cambios se realizaron en un proyecto de aplicación o en un módulo_._  Los cambios se muestran en negrita en el Editor de modelos. Para ver el código XML subyacente, haga clic con el botón derecho en el archivo XAFML y elija  **Ver código**. Estos cambios se superponen a los valores anteriores cargados en el modelo de aplicación. Los valores especificados en el Editor de modelos para el proyecto de aplicación son los valores finales del modelo de aplicación. Se utilizan al iniciar la aplicación. Para obtener información detallada, consulte el tema siguiente:  [Conceptos básicos del modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works).

## Usar acciones para invocar el editor de modelos en tiempo de ejecución

Cuando se concede el  [permiso](https://docs.devexpress.com/eXpressAppFramework/113366/data-security-and-safety/security-system)  **Editar modelo**  o  **Administrativo**  al usuario actual, la categoría  **Herramientas**  de la ventana raíz de la aplicación WinForms muestra las acciones  **EditModel**  y  **ViewInModel**. Estas acciones están disponibles si el  [sistema de seguridad](https://docs.devexpress.com/eXpressAppFramework/113366/data-security-and-safety/security-system)  no está habilitado.

### La acción Editar modelo

Utilice la acción  **EditarModelo**  (o el acceso directo CTRL+MAYÚS+F1) para invocar la ventana Editor de modelos.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/11b741a8-afb9-420c-adfc-bfa831b711e4)


![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/e3242e78-8efb-42e7-a014-c6c40e16bc22)


### La vista en la acción del modelo

Utilice la acción  **VerInModel**  (o el acceso directo CTRL+MAYÚS+F2) para invocar la ventana Editor de modelos con el elemento de nodo Ver seleccionado.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/a72d67d3-afea-4651-bbc3-6f860c003698)


![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/39dfdd58-155d-4b72-85d8-a48ff59f869e)


También puede utilizar elementos de acción para abrir el Editor de modelos con los nodos Clase o Validación seleccionados.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/d5166d28-0f41-41c8-ac24-289f5a947438)


## Almacenamiento de diferencias de modelo de usuario

En tiempo de ejecución, el Editor de modelos contiene información completa que incluye los cambios recientes realizados en la interfaz de usuario. Cuando un usuario realiza cambios en la interfaz de usuario o en el Editor de modelos, se guardan en el archivo  _Model.User.xafml._  Este archivo se encuentra en la carpeta con el archivo ejecutable de la aplicación. Establezca la clave de configuración  **UserModelDiffsLocation**  en  **CurrentUserApplicationDataFolder**  para colocar este archivo en la carpeta  _ApplicationData_  de un usuario.

**Archivo:**  _MySolution.Win\App.config_



```xml
<appSettings>
    <add key="UserModelDiffsLocation" value="CurrentUserApplicationDataFolder"/>
</appSettings>

```

También puede almacenar la configuración del usuario en la base de datos. Consulte el tema siguiente para obtener más información:  [Almacenamientos por diferencias de modelo.](https://docs.devexpress.com/eXpressAppFramework/403527/ui-construction/application-model-ui-settings-storage/change-application-model)

## Implementar y ejecutar el Editor de modelos independiente

El administrador de la aplicación puede ejecutar el Editor de modelos como una utilidad independiente en Windows. El administrador puede editar un modelo de la aplicación de formularios Web Forms ASP.NET implementada o de la aplicación de Windows Forms implementada con la acción  **EditModel**  deshabilitada.

### En .NET Framework

Para utilizar el Editor de modelos independiente, cree una carpeta en una estación de trabajo o un servidor donde se implemente la aplicación XAF. Copie el archivo de configuración %PROGRAMFILES%\DevExpress 23.1\Components\Tools\eXpressAppFramework\Model Editor\DevExpress.ExpressApp.ModelEditor.v 23.1.exe y %PROGRAMFILES%\DevExpress 23.1\_Components\Tools\eXpressAppFramework\Model Editor\DevExpress.ExpressApp.ModelEditor.v  23.1.config_  de la estación de trabajo del desarrollador en la carpeta recién creada_._  Copie los siguientes ensamblados en la misma carpeta:


-   _DevExpress.Data.v23.1.dll_
-   _DevExpress.Data.Desktop.v23.1.dll_
-   _DevExpress.ExpressApp.Win.v23.1.dll_
-   _DevExpress.ExpressApp.Xpo.v23.1.dll_
-   _DevExpress.ExpressApp.v23.1.dll_
-   _DevExpress.Images.v23.1.dll_
-   _DevExpress.Office.v23.1.Core.dll_
-   _DevExpress.Persistent.Base.v23.1.dll_
-   _DevExpress.RichEdit.v23.1.Core.dll_
-   _DevExpress.Utils.v23.1.dll_
-   _DevExpress.Xpo.v23.1.dll_
-   _DevExpress.XtraBars.v23.1.dll_
-   _DevExpress.XtraEditors.v23.1.dll_
-   _DevExpress.XtraLayout.v23.1.dll_
-   _DevExpress.XtraRichEdit.v23.1.dll_
-   _DevExpress.XtraTreeList.v23.1.dll_
-   _DevExpress.XtraVerticalGrid.v23.1.dll_
-   Todos los ensamblados requeridos por la aplicación XAF (implementados junto con el ejecutable WinForms o ubicados en la subcarpeta  _Bin_  de la aplicación ASP.NET formularios Web Forms).

Estos ensamblados están disponibles en la carpeta %_PROGRAMFILES%\DevExpress  23.1\Components\Bin\Framework_  en la estación de trabajo del desarrollador.

>NOTA
Estos ensamblados y ejecutables son [redistribuibles](https://docs.devexpress.com/eXpressAppFramework/113033/deployment/redistribution-and-deployment). Puede registrar los ensamblados necesarios en la  [caché  global de ensamblados](https://docs.microsoft.com/en-us/dotnet/framework/app-domains/gac) (GAC), en lugar de copiarlos en la carpeta Editor de modelos.

Después de invocar el Editor de modelos independiente, utilice el cuadro de diálogo Abrir modelo para especificar el  **modelo**  de aplicación que desea editar. Este cuadro de diálogo está abierto inicialmente. Para volver a invocarlo más tarde, haga clic en el botón  **Abrir modelo**  de la  [barra de herramientas](https://docs.devexpress.com/eXpressAppFramework/113327/ui-construction/application-model-ui-settings-storage/model-editor/menu-toolbars).

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/cc17e13b-32d5-4520-8fdc-2100ecd2dbdc)


### En .NET 6

Para utilizar el Editor de modelos independiente:

1.  Copie la carpeta %_PROGRAMFILES%\DevExpress  23.1\Components\Tools\eXpressAppFrameworkNetCore\Model Editor_  de la estación de trabajo del desarrollador a una estación de trabajo o un servidor donde se implemente la aplicación XAF.
2.  Copie todos los ensamblados necesarios para la aplicación XAF (implementados junto con el ejecutable).

Después de invocar el Editor de modelos independiente, utilice el cuadro de diálogo Abrir modelo para especificar el  **modelo**  de aplicación que desea editar. Este cuadro de diálogo está abierto inicialmente. Para volver a invocarlo más tarde, haga clic en el botón  **Abrir modelo**  de la  [barra de herramientas](https://docs.devexpress.com/eXpressAppFramework/113327/ui-construction/application-model-ui-settings-storage/model-editor/menu-toolbars).

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/7dd65ac5-0b08-4d85-a810-a1fbf75151ae)


### Abrir y editar modelo de aplicación

Para editar el modelo de aplicación de la aplicación implementada, debe especificar únicamente el nombre del archivo de configuración. Una opción para elegir un archivo de ensamblado y una ruta de acceso de diferencias de modelo permite editar el modelo de un módulo específico fuera de Visual Studio.

También puede usar parámetros de línea de comandos para especificar un modelo de aplicación para abrir. El Editor de modelos independiente tiene la siguiente sintaxis de línea de comandos:

`DevExpress.ExpressApp.ModelEditor.v23.1.exe appConfigFile | moduleAssemblyFile diffsPath | /?`

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/cfaeba1f-a22c-4fe1-8a1d-e2315b166075)
![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/a1117c4a-3fcb-4052-a58f-09b32555d8ec)


>NOTA
El archivo de configuración de la aplicación de formularios Windows Forms es:
>-   _<Application_Name>.Ganar.  .exe.  config_ - para .NET Framework
>-   _<Application_Name>.Ganar.  .dll.  config_ - para .NET 6.
>
>El archivo de configuración de la aplicación ASP.NET Web Forms es Web. _config_.
>La aplicación ASP.NET Core Blazor no incluye un archivo de configuración.  Especifique el _archivo de ensamblado del móduloy los parámetros de ruta de diferencias en lugar del archivo de configuración de_  _la aplicación_.

Se invocará el Editor de modelos representado como un formulario.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/4ec65a33-2289-4961-98c1-3d404190c2ec)


Como alternativa, puede hacer clic con el botón secundario en el archivo de configuración en el Explorador de Windows y hacer clic en el  _botón Abrir con..._* para elegir el ejecutable del Editor de modelos.

Para editar el modelo de aplicación de un módulo XAF, escriba la ruta completa del ejecutable del Editor de modelos en el símbolo del sistema y pase el archivo de ensamblado y la carpeta de diferencias como parámetros (consulte la tabla anterior).


# Barras de herramientas de menú


El  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor)  incluye una barra de herramientas que se puede mostrar en tiempo de diseño y en tiempo de ejecución. En este tema se describe la funcionalidad en ambos casos.

## Barra de herramientas en tiempo de diseño

La siguiente imagen ilustra la barra de herramientas que está disponible en tiempo de diseño.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/2adf5f1d-6b5b-470f-9856-08271e16135d)

Para guardar los cambios realizados en el Editor de modelos en tiempo de diseño, use el botón  **Guardar**  de Visual Studio o el acceso directo CTRL+S.

## Barra de herramientas de tiempo de ejecución y menú independiente

La imagen siguiente ilustra la barra de herramientas que está disponible cuando se  [invoca](https://docs.devexpress.com/eXpressAppFramework/113326/ui-construction/application-model-ui-settings-storage/model-editor/ways-to-invoke-the-model-editor)  el Editor de modelos en tiempo de ejecución (en la interfaz de usuario de WinForms de XAF en .NET Framework) o como una herramienta independiente.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/223f90d1-a26c-4008-982e-3d4739b9002b)

## Limitaciones

XAF utiliza una única extensión de Visual Studio para todas las versiones. Como resultado, la barra de herramientas del Editor de modelos no se admite en proyectos .NET creados para versiones anteriores después de instalar XAF v22.1 o posterior.


# Árbol de nodos


El  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor)  proporciona una lista de árbol que representa la estructura del modelo de aplicación. Puede utilizar este árbol para navegar a un nodo determinado y modificar la estructura del modelo de aplicación añadiendo, quitando y reorganizando nodos. En este tema se proporciona información sobre las siguientes capacidades y características del árbol de nodos.

-   [Visualización de la estructura del modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/113328/ui-construction/application-model-ui-settings-storage/model-editor/node-tree#applicationmodelstructurevisualization)
-   [Comandos del menú contextual](https://docs.devexpress.com/eXpressAppFramework/113328/ui-construction/application-model-ui-settings-storage/model-editor/node-tree#contextmenucommands)
-   [Funcionalidad de arrastrar y soltar](https://docs.devexpress.com/eXpressAppFramework/113328/ui-construction/application-model-ui-settings-storage/model-editor/node-tree#draganddropfunctionality)
-   [Nodos vinculados](https://docs.devexpress.com/eXpressAppFramework/113328/ui-construction/application-model-ui-settings-storage/model-editor/node-tree#linkednodes)
-   [Agrupación](https://docs.devexpress.com/eXpressAppFramework/113328/ui-construction/application-model-ui-settings-storage/model-editor/node-tree#grouping)

## Visualización de la estructura del modelo de aplicación

Cada nodo tiene un título especificado por su propiedad  **Id**  y una imagen asociada. Los nodos que se modifican en la capa del modelo actual tienen títulos que se muestran en negrita. Las propiedades del nodo actualmente enfocado se muestran en la  [cuadrícula de propiedades](https://docs.devexpress.com/eXpressAppFramework/113329/ui-construction/application-model-ui-settings-storage/model-editor/property-grid)  a la derecha.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/e37f0393-e090-4f92-881d-8062235534da)

## Comandos del menú contextual

El siguiente menú contextual está disponible al hacer clic con el botón secundario en un nodo.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/2bf1df36-a85c-4b66-9704-c625964085e2)

La mayoría de los comandos disponibles en este menú se pueden aplicar no solo a un solo nodo enfocado, sino a varios nodos seleccionados a la vez. Para seleccionar varios nodos, utilice el enfoque estándar: mantenga pulsada la tecla MAYÚS y haga clic en un nodo para seleccionar nodos secuenciales, o mantenga pulsada la tecla CTRL y haga clic en un nodo para seleccionar nodos individuales. Los comandos del menú contextual se describen en la tabla siguiente.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/b4f1a451-a03f-4a59-8386-915024e52e87)
![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/79842e43-a409-40c8-99bc-ad5089b2fa7f)


## Funcionalidad de arrastrar y soltar

-   Puede mover un nodo secundario de un nodo primario a otro mediante arrastrar y soltar. Es conveniente mover una acción a otro contenedor de acciones, mover un elemento de navegación a otro grupo y modificar el diseño de la vista detallada. La flecha amarilla a la izquierda indica el nodo de destino.
    
    ![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/3651290a-3e58-4206-82ae-ab7918b61b54)

    
    Al arrastrar, puede colocar un puntero sobre el nodo de destino y se expandirá automáticamente después de un pequeño retraso. Para crear una copia del nodo en lugar de moverlo, simplemente mantenga presionada la tecla CTRL al arrastrar. Si el nodo de destino ya tiene un hijo con el mismo ID, se agregará el sufijo "_Copy".
    
-   Puede reorganizar los elementos secundarios de un determinado nodo principal mediante arrastrar y soltar. Es conveniente al reordenar columnas de vista de lista o elementos de navegación. Mantenga pulsada la tecla MAYÚS y arrastre un nodo secundario hacia arriba o hacia abajo en la lista dentro de los límites de su nodo principal. La flecha azul a la izquierda indica la nueva posición del nodo.
    
    ![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/22c54f88-cba1-42f9-a1db-811b50c68651)

    
    Los índices de los nodos secundarios se modificarán automáticamente para que correspondan a la reorganización.
    

Puede seleccionar varios nodos y arrastrar y soltar varios nodos a la vez.

## Nodos vinculados

Algunos nodos tienen un nodo secundario "virtual" denominado  **Links**. Está oculto por defecto; puede alternar su visibilidad mediante el  [comando](https://docs.devexpress.com/eXpressAppFramework/113327/ui-construction/application-model-ui-settings-storage/model-editor/menu-toolbars)  **Mostrar vínculos / Ocultar vínculos**. En este nodo, puede ver nodos que contienen referencias al nodo actual. La captura de pantalla siguiente ilustra el nodo  **Departamento**  del tipo  [IModelClass](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelClass). En el nodo  **Vínculos**, puede ver el  **elemento Creatable**  para el objeto Departamento, el tipo  **Miembros**  del departamento y las  **Vistas**  diseñadas para el tipo Departamento.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/3d8b4fa9-023f-4802-9b00-28f557e70058)


Puede editar las propiedades de los nodos vinculados en su lugar o navegar a una ubicación de nodo real mediante el comando  **Ir al origen**.

## Agrupación

Utilice el comando del menú contextual  **Agrupar/Desagrupar**  para activar/desactivar la agrupación. La siguiente captura de pantalla ilustra el nodo  **Vistas**  agrupadas.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/a2463893-f74a-4688-b16c-40554294c364)


Las vistas que no dependen de una determinada clase empresarial (por ejemplo, DashboardView) pertenecen al nodo  **No especificado**.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/9bd689b0-eeda-46bb-a0b9-443031d9a09e)


De forma predeterminada, se admite la agrupación para los nodos secundarios de  **Vistas**,  **BOModel**,  **Controladores**  y  **Acciones**. Para personalizar la agrupación predeterminada o implementar reglas de agrupación para otros nodos, utilice la clase  [ModelEditorGroupingHelper](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ModelEditor.ModelEditorGroupingHelper).


# Cuadrícula de propiedades


El  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor)  proporciona la cuadrícula de propiedades que se muestra a la derecha del  [árbol de nodos](https://docs.devexpress.com/eXpressAppFramework/113328/ui-construction/application-model-ui-settings-storage/model-editor/node-tree). En este tema se proporciona información sobre las siguientes capacidades y características de esta cuadrícula.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/47398059-d70b-4c06-aa14-2a9b21e3a8b5)


La cuadrícula de propiedades permite personalizar los valores de propiedad para el nodo enfocado. Dependiendo de un tipo de propiedad, puede editar su valor en un cuadro de texto, seleccionar uno de los valores predefinidos en una lista desplegable o utilizar un  [editor mejorado](https://docs.devexpress.com/eXpressAppFramework/113330/ui-construction/application-model-ui-settings-storage/model-editor/enhanced-editors)  (invocable en una ventana emergente a través del botón de puntos suspensivos). También puede editar los valores de propiedad de varios nodos al mismo tiempo. Seleccione varios nodos en el árbol de nodos. La cuadrícula de propiedades contendrá sus propiedades comunes. Si cambia algunos valores, los cambios se aplicarán a todos los nodos seleccionados.

## Visualización de propiedades

La cuadrícula de propiedades está categorizada y puede contraer y expandir categorías, o cambiar a la ordenación alfabética. Se resalta una propiedad enfocada. Los valores modificados se muestran en negrita. Las celdas de cuadrícula tienen diferentes colores y estilos de fuente, por lo que puede distinguir fácilmente entre propiedades de diferentes tipos. Algunos de los tipos de propiedad tienen imágenes asignadas con el propósito de una mejor visualización. Los siguientes tipos están disponibles.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/6602fdf8-90b0-4c98-af96-cb2fb1c3432a)


Una propiedad puede adaptarse a varios de estos tipos. En este caso, los márgenes se combinan. Por ejemplo, la propiedad  **Id**  en la imagen anterior es obligatoria y es de solo lectura. Por lo tanto, su fuente es gris, negrita y cursiva.

## Mini-barra de herramientas

La minibarra de herramientas que se muestra encima de la cuadrícula de propiedades proporciona los siguientes botones.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/fa7a72b3-d639-41d4-b8e4-5f9c8c264417)


## Validación de valores de propiedad

Se validan los valores de propiedad requeridos. Si crea un nodo, deja vacío su valor de propiedad requerido e intenta navegar a otro nodo, se mostrará la advertencia de validación. Para resolver un problema, debe proporcionar el valor requerido o eliminar el nodo actual si se creó por error.


# Editores de propiedades de nodo


En el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor), los valores de propiedad de cadena mostrados en la  [cuadrícula de propiedades](https://docs.devexpress.com/eXpressAppFramework/113329/ui-construction/application-model-ui-settings-storage/model-editor/property-grid)  se pueden editar mediante un editor de cadenas simple. Sin embargo, hay ciertas propiedades complejas cuyos valores son difíciles de editar manualmente. En estos casos, se muestra un botón de puntos suspensivos (![EllipsisButton](https://docs.devexpress.com/eXpressAppFramework/images/ellipsisbutton116182.png)) a la derecha de la propiedad. Al hacer clic en este botón, se invoca un  _editor mejorado_, que se muestra en una ventana emergente. En este tema se enumeran los editores de propiedades mejorados disponibles en el  **Editor de modelos**.

-   [Selector de imágenes](https://docs.devexpress.com/eXpressAppFramework/113330/ui-construction/application-model-ui-settings-storage/model-editor/enhanced-editors#imagepicker)
-   [Editor de expresiones](https://docs.devexpress.com/eXpressAppFramework/113330/ui-construction/application-model-ui-settings-storage/model-editor/enhanced-editors#expressioneditor)
-   [Generador de filtros](https://docs.devexpress.com/eXpressAppFramework/113330/ui-construction/application-model-ui-settings-storage/model-editor/enhanced-editors#filterbuilder)
-   [Editor de máscaras](https://docs.devexpress.com/eXpressAppFramework/113330/ui-construction/application-model-ui-settings-storage/model-editor/enhanced-editors#maskeditor)
-   [Diseñador de gráficos](https://docs.devexpress.com/eXpressAppFramework/113330/ui-construction/application-model-ui-settings-storage/model-editor/enhanced-editors#chartdesigner)
-   [Diseñador de cuadrícula dinámica](https://docs.devexpress.com/eXpressAppFramework/113330/ui-construction/application-model-ui-settings-storage/model-editor/enhanced-editors#pivotgrid-designer)

## Selector de imágenes

El  **Selector**  de imágenes está disponible para las propiedades que hacen referencia a imágenes y especifican nombres de imagen (por ejemplo, IModelClass.ImageName e  [IModelAction.ImageMode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelAction.ImageMode)).  [](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelClass.ImageName)Si no conoce el nombre exacto de la imagen, haga clic en el botón de puntos suspensivos (![EllipsisButton](https://docs.devexpress.com/eXpressAppFramework/images/ellipsisbutton116182.png)) que se muestra a la derecha del nombre de una imagen y examine las imágenes disponibles en el cuadro de diálogo  **Selector de imágenes**.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/75d7e55f-9ea4-41c8-876a-c0dcd1cee327)


>NOTA
El Selector de imágenes enumera las imágenes disponibles en [los orígenes de imágenes](https://docs.devexpress.com/eXpressAppFramework/112792/application-shell-and-base-infrastructure/icons/add-and-replace-icons)  actuales. No puede agregar imágenes personalizadas dentro de este cuadro de diálogo. Para agregar su propia imagen, siga el enfoque descrito en el siguiente artículo:  [Cómo: Asignar imágenes personalizadas a clases empresariales](https://docs.devexpress.com/eXpressAppFramework/404209/application-shell-and-base-infrastructure/icons/how-to-assign-custom-images-to-business-classes).

## Editor de expresiones

El  **Editor**  de expresiones está disponible para las propiedades que  [especifican expresiones (por](https://docs.devexpress.com/CoreLibraries/4928/devexpress-data-library/criteria-language-syntax)  ejemplo,  [IModelMember.Expression](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelMember.Expression)). Si no desea escribir una expresión manualmente, haga clic en el botón de puntos suspensivos (![EllipsisButton](https://docs.devexpress.com/eXpressAppFramework/images/ellipsisbutton116182.png)) que se muestra a la derecha de un valor de expresión y utilice el cuadro de diálogo  [Editor de expresiones](https://docs.devexpress.com/WindowsForms/6212/common-features/expressions/expression-editor)  invocado para seleccionar funciones, operadores y operandos.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/b0fd5fb1-881a-46d3-b94c-7a52fb7ed125)


## Generador de filtros

El  **Generador de filtros**  está disponible para las propiedades que especifican  [criterios](https://docs.devexpress.com/CoreLibraries/4928/devexpress-data-library/criteria-language-syntax)  (por ejemplo, IModelListView.Criteria,  [IModelListViewFilterItem.Criteria](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelListViewFilterItem.Criteria)  e  [IRuleBaseProperties.TargetCriteria](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Validation.IRuleBaseProperties.TargetCriteria)).  [](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelListView.Criteria)Puede invocar el cuadro de diálogo  **Generador de filtros**  para diseñar visualmente el criterio requerido haciendo clic en el botón de puntos suspensivos (![EllipsisButton](https://docs.devexpress.com/eXpressAppFramework/images/ellipsisbutton116182.png)) que se muestra a la derecha de un criterio.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/7292cd9c-0e9b-4de3-9fe6-1a2ce8ae6df5)


## Editor de máscaras

El  **Editor**  de máscaras está disponible para las propiedades que especifican  [Editar máscaras](https://docs.devexpress.com/WindowsForms/583/controls-and-libraries/editors-and-simple-controls/common-editor-features-and-concepts/input-mask)  (por ejemplo,  [IModelCommonMemberViewItem.EditMask](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelCommonMemberViewItem.EditMask)). Si no desea escribir una máscara manualmente, haga clic en el botón de puntos suspensivos (![EllipsisButton](https://docs.devexpress.com/eXpressAppFramework/images/ellipsisbutton116182.png)) que se muestra a la derecha de un valor de máscara. El cuadro de diálogo proporciona varias máscaras predefinidas. También es posible crear máscaras personalizadas. El cuadro de diálogo  **Probar entrada**  le permite probar la máscara seleccionada.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/4a4aa76d-c1be-4861-899f-12150f0055c6)


## Diseñador de gráficos

El  **Diseñador**  de gráficos está diseñado para especificar la configuración del Editor de listas de gráficos ([IModelChartSettings.Settings](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Chart.IModelChartSettings.Settings)). Para invocar al diseñador, haga clic en el botón de puntos suspensivos (![EllipsisButton](https://docs.devexpress.com/eXpressAppFramework/images/ellipsisbutton116182.png)) que se muestra a la derecha de una cadena de configuración. Para ver un ejemplo de cómo usar este diseñador, consulte el tutorial  [Cómo: Mostrar una vista de lista como gráfico](https://docs.devexpress.com/eXpressAppFramework/113314/ui-construction/list-editors/how-to-display-a-list-view-as-a-chart). Para obtener información detallada sobre las capacidades del Diseñador de gráficos, consulte el tema  [Diseñador de gráficos](https://docs.devexpress.com/WindowsForms/114070/controls-and-libraries/chart-control/design-time-features/chart-designer).

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/02d4aded-8b71-4fd0-abb0-c6850ad5e8f8)


## Diseñador de cuadrícula dinámica

El  **Diseñador de cuadrícula dinámica**  está diseñado para especificar la configuración del Editor de listas de cuadrículas dinámicas ([IPivotSettings.Settings](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.PivotGrid.IPivotSettings.Settings)). Para invocar el asistente, haga clic en el botón de puntos suspensivos (![EllipsisButton](https://docs.devexpress.com/eXpressAppFramework/images/ellipsisbutton116182.png)) que se muestra a la derecha de una cadena de configuración. Para obtener información detallada sobre las capacidades del Diseñador de cuadrícula dinámica, consulte el tema de ayuda del  [Diseñador de cuadrícula dinámica](https://docs.devexpress.com/WindowsForms/1825/controls-and-libraries/pivot-grid/design-time-features/pivotgrid-designer).

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/262e84cf-521c-4692-9aa5-836bfb3711f0)


# Panel de búsqueda

En aplicaciones XAF simples, es relativamente fácil navegar por el árbol del  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor). Sin embargo, a medida que la aplicación se vuelve más compleja y crece el número de nodos del modelo de aplicación, encontrar el nodo necesario en el árbol del editor de modelos puede ser más difícil. En este caso, se recomienda el uso de la característica de búsqueda de nodos descrita en este tema.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/646ebcd8-c556-4038-ba8f-a57b94c16398)

Este panel se puede mostrar presionando CTRL+ALT+F o haciendo clic en el botón  **Buscar**  de la  [barra de herramientas del Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/113327/ui-construction/application-model-ui-settings-storage/model-editor/menu-toolbars).

El panel Buscar consta de tres secciones:  **Buscar, Esquema**  de  **modelo**  y  **Resultados de búsqueda**.

La sección  **Buscar**  contiene el cuadro combinado de búsqueda. Escriba el texto que desea encontrar y el contenido de  **los resultados**  de la búsqueda se actualizará instantáneamente a medida que escriba. El cuadro combinado de búsqueda almacena todas las búsquedas anteriores. Se puede acceder a ellos haciendo clic en el botón desplegable. El primer clic en el botón desplegable después de que se haya insertado el texto invoca un menú desplegable que muestra una lista de búsquedas anteriores que comienzan con caracteres escritos. El segundo clic en el botón desplegable invoca un menú desplegable con una lista de todas las búsquedas anteriores.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/8978dcbc-0358-4ca4-a27d-7997ccf51933)

La sección  **Esquema del modelo**  especifica sobre qué nodos se realizará una búsqueda. Utilice el botón  **Ocultar/Mostrar esquema**  para alternar la visibilidad de la sección Esquema del modelo. De forma predeterminada, todos los nodos están seleccionados. Sin embargo, puede optar por buscar solo en los nodos necesarios.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/1c17ce90-5c88-4723-a124-712ecfd23308)

La sección Resultados de la búsqueda muestra los nodos que se encontraron durante  **la búsqueda**. Seleccione cualquiera de estos nodos para enfocar el mismo nodo en el  [árbol de nodos](https://docs.devexpress.com/eXpressAppFramework/113328/ui-construction/application-model-ui-settings-storage/model-editor/node-tree).


# Nodos inutilizables

En XAF, un  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)  tiene una  [estructura en capas](https://docs.devexpress.com/eXpressAppFramework/403527/ui-construction/application-model-ui-settings-storage/change-application-model). Los generadores y actualizadores de modelos crean la primera capa basada en el modelo de Business Objects actual. Esta capa es una estructura de aplicación base sin diferencias. Consulte el tema Modelo  [de negocio en el Modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)  para obtener información adicional.

Las capas superiores se crean en función de la configuración personalizada del modelo a partir de los archivos XAFML de los módulos y las aplicaciones y las diferencias de modelo de un usuario (desde un archivo XAFML o una base de datos). Si la configuración de una capa superior no se puede aplicar a un nodo especificado desde la primera capa (el nodo con este nombre no existe), esta configuración se reemplaza en el archivo  _UnusableNodes.xafml._

Si la aplicación contiene nodos inutilizables, muestra el siguiente mensaje de advertencia al personalizar el modelo de aplicación mediante el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor):

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/6756a47b-c92a-4bcb-8c0f-deec76979738)

También puede mostrar nodos inutilizables en el Editor de modelos haciendo clic en el botón  **Mostrar nodos inutilizables**.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/5e3fee00-a873-4ecb-89f4-883e514e48d1)

## Cómo funciona

La configuración se puede reemplazar en el archivo  _UnusableNodes.xafml_  en los siguientes casos:

-   Se han producido cambios en el esquema predeterminado del modelo de aplicación y el nuevo esquema no reconoce los nodos.
    
    _Ejemplo_:
    
    En el esquema anterior, el atributo "A" se colocaba en el nodo "B" del nodo  **BOModel**. En la nueva versión XAF, el atributo "A" se define en el nodo "C" del nodo  **ListView**  debido a un cambio importante. Si no hay un convertidor especial de diferencias de modelo, las personalizaciones anteriores que afectan al atributo "A" se reemplazan en el archivo  _UnusableNodes_.
    
-   Hubo cambios en la estructura de sus clases de negocios con respecto a cambiar el nombre y eliminar atributos de una clase y sus miembros, así como declarar nuevos miembros y eliminar miembros anteriores en la clase.
    
    _Ejemplo_:
    
    Ha cambiado el nombre de la clase y ha perdido todos los cambios realizados en su nodo de clase en la  **lista de materiales**, los nodos de vistas correspondientes en el nodo  **Vistas**, etc.
    

## Ejemplo

En esta sección se explica en qué caso se mueve la configuración al archivo  _UnusableNodes.xafml_  y cómo puede restaurarla. La configuración de los elementos de navegación se utiliza aquí con fines de demostración.

En XAF, los elementos de navegación se generan a partir de clases de negocio marcadas con  [DefaultClassOptionsAttribute](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.DefaultClassOptionsAttribute)  o  [NavigationItemAttribute](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.NavigationItemAttribute). Los elementos de navegación decorados con uno de estos atributos se colocan en el grupo de navegación correspondiente. El atributo  **DefaultClassOptions**  coloca el elemento de navegación en el grupo  **Default**. El atributo  **NavigationItem**  permite especificar el nombre del grupo de destino (para obtener más información, consulte el tema de la clase de atributo.

Puede modificar la colección de elementos de navegación de las siguientes maneras:

1.  Modifique los elementos de navegación creados.
2.  Agregue un nuevo elemento de navegación a los grupos existentes que un generador creó automáticamente.
3.  Agregue un nuevo elemento de navegación al grupo de navegación raíz o cree un nuevo grupo de navegación y agréguele un elemento.

A continuación, si quita  **DefaultClassOptionsAttribute**  y  **NavigationItemAttribute**  de todas las clases empresariales, todas las diferencias que se aplicaron a la primera y segunda forma se mueven al archivo  _UnusableNodes.xafml._  Esto ocurre porque los elementos de navegación que se crearon para las clases decoradas con estos atributos se quitan del modelo de aplicación. Por lo tanto, estas diferencias no se pueden aplicar a los nodos correspondientes. Estas diferencias se guardan en un archivo XAFML como diferencias aplicadas al nodo  **NavigationItems**  predeterminado porque había clases marcadas con el atributo  **DefaultClassOptions**  o  **NavigationItem**.

Si no hay clases marcadas con estos atributos en una solución y sus módulos, los nodos de grupo con el nombre  **DefaultGroupName**  y los nombres del atributo  **NavigationItem**  no se generan de forma predeterminada y las diferencias de modelo no se pueden aplicar a estos nodos. Tenga en cuenta que las diferencias de modelo solo se aplican a los nodos existentes.

Siga los pasos a continuación para restaurar la configuración perdida:

1.  Abra el Editor de modelos, haga clic con el botón derecho en el nodo raíz de la  **aplicación**  para agregar los  **NavigationItems**, cuya configuración se movió al archivo  _UnusableNodes_, y guarde los cambios.
2.  Abra el archivo  _UnusableNodes_  en un editor de texto y copie ciertas personalizaciones. Abra el archivo XAFML de diferencias de modelo requerido como un archivo de texto y pegue esta configuración en él.
3.  Inicie la aplicación y asegúrese de que la configuración reemplazada se aplicó correctamente.
4.  Quite el archivo  _UnusableNodes_.

>NOTA
Puede usar el enfoque descrito en el tema  [Convertir diferencias de modelo](https://docs.devexpress.com/eXpressAppFramework/112796/ui-construction/application-model-ui-settings-storage/application-model-storages/handle-application-model-upgrades)  de aplicación  para resolver automáticamente el problema con la pérdida de personalizaciones después de migrar a una nueva versión de la aplicación.


# Configuración del Editor de modelos

El Editor de modelos guarda su configuración entre ejecuciones (el nodo actualmente enfocado, la configuración del panel de búsqueda, etc.). En este tema se enumeran los almacenamientos de configuración utilizados por el Editor de modelos en diferentes circunstancias.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/ba7d729f-6a4e-46b3-9567-483277cf07cc)

# Personalizar el modelo de aplicación en el código


# Leer y establecer valores para nodos de modelo de aplicación integrados en código


El  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112579/ui-construction/application-model-ui-settings-storage)  tiene una estructura de árbol. Puede iterar a través del árbol para acceder al nodo requerido. Para tener acceso al nodo raíz Application Model, utilice la propiedad  [XafApplication.Model.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.XafApplication.Model)

Las siguientes clases XAF también tienen la propiedad  **Model**  para proporcionar acceso al nodo correspondiente en el modelo de aplicación:

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/ee42d98c-231b-432a-baf9-c6f14c4d5f87)


Los nodos del modelo de aplicación implementan la interfaz  [IModelNode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelNode). Puede usar  [miembros de IModelNode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelNode._members)  para obtener nodos primarios y secundarios.

Los nodos del modelo de aplicación también implementan otras interfaces con propiedades específicas. Para tener acceso a las propiedades específicas de un nodo determinado, convierta este nodo en una de las interfaces que implementa. Por ejemplo, el nodo Aplicación raíz implementa la interfaz  **del nodo**  [IModelApplication](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelApplication). Puede convertir este nodo al tipo IModelApplication y tener acceso a la propiedad  [IModelApplication.Logo.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelApplication.Logo)  [](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelApplication)También puede convertir el nodo Application al tipo  [IModelApplicationNavigationItems](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelApplicationNavigationItems)  y tener acceso a la propiedad  [NavigationItems](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelApplicationNavigationItems.NavigationItems).  Consulte el tema siguiente para obtener más información sobre las interfaces disponibles:  [Modelo de aplicación: interfaces integradas](https://docs.devexpress.com/eXpressAppFramework/403535/ui-construction/application-model-ui-settings-storage/how-application-model-works/application-model-interfaces).

## Ejemplos

Los ejemplos siguientes realizan cambios en la capa superior del modelo de aplicación (la capa de personalización del usuario). Utilice las técnicas del tema siguiente para personalizar la capa cero subyacente:  **Leer y establecer valores para nodos integrados del modelo de aplicación en el código**.

### Personalizar un nodo de aplicación raíz

En el ejemplo siguiente se obtiene acceso al nodo Application raíz y se utiliza la propiedad  [IModelRootNavigationItems.StartupNavigationItem](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelRootNavigationItems.StartupNavigationItem)  para mostrar la vista  _Department_ListView_  al inicio de la aplicación.

```csharp
using DevExpress.ExpressApp.SystemModule;  
using System.Linq;  
// ...  
    private void MainDemoWinApplication_LoggedOn(object sender, LogonEventArgs e) {  
        XafApplication app = (XafApplication)sender;  
        IModelApplicationNavigationItems navigationItems = (IModelApplicationNavigationItems)app.Model;  
        string targetViewId = "Department_ListView";  
        IModelNavigationItem targetNavigationItem = navigationItems.NavigationItems.AllItems.FirstOrDefault(
            item => item.View?.Id == targetViewId);
        if(targetNavigationItem != null) {  
            navigationItems.NavigationItems.StartupNavigationItem = targetNavigationItem;  
        }  
    }  
// ...  

```

### Personalizar un nodo de objeto de negocio

El código siguiente muestra cómo acceder a la clase de negocio  _Contact_  y cambiar su  [título](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelClass.Caption):

```csharp
using System.Linq;
using DevExpress.ExpressApp;
using DevExpress.ExpressApp.Actions;

namespace YourSolutionName.Module.Controllers {
    public class CustomController : ViewController {
        public CustomController() {
            var myAction1 = new SimpleAction(this, "MyAction1", null);
            myAction1.Execute += MyAction1_Execute;
        }

        private void MyAction1_Execute(object sender, SimpleActionExecuteEventArgs e) {
            var bo = Application.Model.BOModel.GetClass(typeof(Contact));
            if(bo != null) {
                var oldCaption = bo.Caption;
                bo.Caption = "New test caption";
            }
        }
    }
}

```

## Limitaciones

La interfaz de usuario de XAF no refleja los cambios realizados en el modelo de aplicación después de la creación del control. Personalice el modelo de aplicación antes de crear y cargar el control utilizado para mostrar un elemento de interfaz de usuario de destino. Obtener acceso al control directamente para personalizar un elemento de la interfaz de usuario después de la creación del control. Consulte el tema siguiente para obtener más información:  [Cómo: Obtener acceso al componente de cuadrícula en una vista de lista](https://docs.devexpress.com/eXpressAppFramework/402154/ui-construction/list-editors/how-to-access-list-editor-control).

También puede volver a crear el control con los cambios más recientes del modelo de aplicación, tal como se describe en el tema siguiente:  [Aplicar cambios en el modelo de aplicación a la vista actual inmediatamente](https://docs.devexpress.com/eXpressAppFramework/118592/ui-construction/application-model-ui-settings-storage/customize-application-model-in-code/apply-application-model-changes-to-the-current-view-immediately).


# Cómo: Extender y obtener acceso a los nodos del modelo de aplicación desde controladores


Siga las instrucciones siguientes para ampliar el  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)  de las dos maneras siguientes:

-   Agregar la propiedad a  **Vistas**  |  **_<ListView>_**  nodos. Si se establece en , la vista de lista correspondiente muestra un pie de página de grupo. `IsGroupFooterVisible``true`
-   Agregar la propiedad a los nodos de columna (**ListView**  |  **Columnas**  |  **Columna**). Esta propiedad especifica el tipo de resumen de grupo para la columna correspondiente.`GroupFooterSummaryType`

Para obtener más información sobre cómo extender el modelo de aplicación, consulte el tema siguiente:  [Extender y personalizar el modelo de aplicación en código](https://docs.devexpress.com/eXpressAppFramework/112810/ui-construction/application-model-ui-settings-storage/customize-application-model-in-code/access-the-application-model-in-code).

[Ver ejemplo: Cómo ampliar el modelo de aplicación](https://github.com/DevExpress-Examples/XAF_how-to-extend-the-application-model-e213)

## Pasos comunes

Implemente interfaces que expongan las propiedades y:`IsGroupFooterVisible``GroupFooterSummaryType`


```csharp
using DevExpress.Data;
using System.ComponentModel;
//...
public interface IModelListViewExtender {
    bool IsGroupFooterVisible { get; set; }
}
public interface IModelColumnExtender {
    [DefaultValue(SummaryItemType.None)]
    SummaryItemType GroupFooterSummaryType { get; set; }
}

```

Reemplace el método  [ModuleBase.ExtendModelInterfaces](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ModuleBase.ExtendModelInterfaces(DevExpress.ExpressApp.Model.ModelInterfaceExtenders))  del módulo base para ampliar el modelo de aplicación con interfaces declaradas:



```csharp
using DevExpress.ExpressApp.Model;
// ...
public sealed partial class ExtendModelModule : ModuleBase {
    // ...
    public override void ExtendModelInterfaces(ModelInterfaceExtenders extenders) {
        base.ExtendModelInterfaces(extenders);
        extenders.Add<IModelListView, IModelListViewExtender>();
        extenders.Add<IModelColumn, IModelColumnExtender>();
    }
}

```

## Pasos específicos de formularios de Win

Herede un  [WinGroupFooterViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController)  del tipo del módulo de formularios Windows Forms. Esta opción de clase base garantiza que el controlador se active solo en vistas de lista.`ViewController<ListView>`

Reemplace el método protegido. Si la propiedad se establece en para la vista de lista actual, muestre el pie de página del grupo. `OnViewControlsCreated``IsGroupFooterVisible``true`

Tenga en cuenta que los usuarios pueden cambiar los tipos de resumen de columna en la interfaz de usuario. Controle el evento  [View.ModelSaved](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.View.ModelSaved)  para guardar estos cambios en el modelo de aplicación cuando los usuarios cierren la vista de lista actual.

```csharp
using System;
using DevExpress.Data;
using DevExpress.ExpressApp;
using DevExpress.ExpressApp.Model;
using DevExpress.ExpressApp.Win.Editors;
using DevExpress.XtraGrid.Views.Grid;
using DevExpress.XtraGrid.Columns;
//...
public class WinGroupFooterViewController : ViewController<ListView> {
    private void View_ModelSaved(object sender, EventArgs e) {
        IModelListViewExtender modelListView = View.Model as IModelListViewExtender;
        if(modelListView != null && modelListView.IsGroupFooterVisible) {
            GridListEditor gridListEditor = View.Editor as GridListEditor;
            if(gridListEditor != null) {
                GridView gridView = gridListEditor.GridView;
                for(int i = 0; i < gridView.GroupSummary.Count; i++) {
                    IModelColumnExtender modelColumn = View.Model.Columns[
                        gridView.GroupSummary[i].FieldName] as IModelColumnExtender;
                    if(modelColumn != null) {
                        modelColumn.GroupFooterSummaryType = gridView.GroupSummary[i].SummaryType;
                    }
                }
            }
        }    
    }
    protected override void OnViewControlsCreated() {
        base.OnViewControlsCreated();
        IModelListViewExtender modelListView = View.Model as IModelListViewExtender;
        if(modelListView != null && modelListView.IsGroupFooterVisible) {
            GridListEditor gridListEditor = View.Editor as GridListEditor;
            if(gridListEditor != null) {
                GridView gridView = gridListEditor.GridView;
                gridView.OptionsView.GroupFooterShowMode = GroupFooterShowMode.VisibleAlways;
                foreach(IModelColumn modelColumn in View.Model.Columns) {
                    IModelColumnExtender modelColumnExtender = modelColumn as IModelColumnExtender;
                    if(modelColumnExtender != null && 
                        modelColumnExtender.GroupFooterSummaryType != SummaryItemType.None) {
                            GridColumn gridColumn = gridView.Columns[
                                modelColumn.ModelMember.MemberInfo.BindingName];
                            gridView.GroupSummary.Add(modelColumnExtender.GroupFooterSummaryType, 
                                modelColumn.Id, gridColumn);
                    }
                }
            }
        }
    }
    protected override void OnActivated() {
        base.OnActivated();
        View.ModelSaved += View_ModelSaved;
    }
    protected override void OnDeactivated() {
        View.ModelSaved -= View_ModelSaved;
        base.OnDeactivated();
    }
}

```

## Pasos específicos de formularios Web Forms de ASP.NET

Herede un  [WebGroupFooterViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController)  del tipo de ASP. Módulo NET Web Forms. Esta opción de clase base garantiza que este controlador se active solo en vistas de lista.`ViewController<ListView>`

Reemplace el método protegido. Si la propiedad se establece en para la vista de lista actual, muestre el pie de página del grupo. `OnViewControlsCreated``IsGroupFooterVisible``true`

Los usuarios no pueden realizar cambios en la configuración de resumen de columna en ASP.NET interfaz de usuario de formularios Web Forms. No es necesario que maneje el evento.`ModelSaved`



```csharp
using System;
using DevExpress.Data;
using DevExpress.ExpressApp;
using DevExpress.ExpressApp.Model;
using DevExpress.ExpressApp.Web.Editors.ASPx;
using DevExpress.Web;
//...
public class WebGroupFooterViewController : ViewController<ListView> {
    protected override void OnViewControlsCreated() {
        base.OnViewControlsCreated();
        IModelListViewExtender modelListView = View.Model as IModelListViewExtender;
        if(modelListView != null && modelListView.IsGroupFooterVisible) {
            ASPxGridListEditor gridListEditor = View.Editor as ASPxGridListEditor;
            if(gridListEditor != null) {
                ASPxGridView gridView = gridListEditor.Grid;
                gridView.Settings.ShowGroupFooter = GridViewGroupFooterMode.VisibleAlways;
                foreach(IModelColumn modelColumn in View.Model.Columns) {
                    IModelColumnExtender modelColumnExtender = modelColumn as IModelColumnExtender;
                    if(modelColumnExtender != null && 
                        modelColumnExtender.GroupFooterSummaryType != SummaryItemType.None) {
                        string fieldName = modelColumn.ModelMember.MemberInfo.BindingName.ToUpper();
                        ASPxSummaryItem summaryItem = null;
                        foreach(ASPxSummaryItem currentItem in gridView.GroupSummary) {
                            if(currentItem.FieldName.ToUpper() == fieldName) {
                                currentItem.ShowInGroupFooterColumn = modelColumn.Caption;
                                summaryItem = currentItem;
                                break;
                            }
                        }
                        if(summaryItem == null) {
                            summaryItem = new ASPxSummaryItem(fieldName, modelColumnExtender.GroupFooterSummaryType);
                            summaryItem.ShowInGroupFooterColumn = modelColumn.Caption.ToUpper();
                            gridView.GroupSummary.Add(summaryItem);
                        }
                    }
                }
            }
        }
    }
}

```

## Pasos específicos de ASP.NET  Core Blazor

Herede un  [WebGroupFooterViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController)  del tipo del módulo Core Blazor de ASP.NET. Esta opción de clase base garantiza que el controlador se active solo en vistas de lista.`ViewController<ListView>`

Reemplace el método protegido. Si la propiedad se establece en para la vista de lista actual, muestre el pie de página del grupo. `OnViewControlsCreated``IsGroupFooterVisible``true`

Los usuarios no pueden personalizar la configuración de resumen en ASP.NET interfaz de usuario de Core Blazor. No es necesario que maneje el evento.`ModelSaved`


```csharp
using DevExpress.Data;
using DevExpress.Data.Summary;
using DevExpress.ExpressApp;
using DevExpress.ExpressApp.Blazor.Editors;
using DevExpress.ExpressApp.Model;
// ...
public class BlazorGroupFooterViewController : ViewController<ListView> {
     protected override void OnViewControlsCreated() {
         base.OnViewControlsCreated();
         if(View.Model is IModelListViewExtender modelListView) {
             if(modelListView.IsGroupFooterVisible && View.Editor is DxGridListEditor gridListEditor) {
                 IDxGridAdapter dataGridAdapter = gridListEditor.GetGridAdapter();
                 dataGridAdapter.GridModel.GroupFooterDisplayMode = DevExpress.Blazor.GridGroupFooterDisplayMode.Always;
                 foreach(IModelColumn modelColumn in View.Model.Columns) {
                     IModelColumnExtender modelColumnExtender = modelColumn as IModelColumnExtender;
                     if(modelColumnExtender != null &&
                         modelColumnExtender.GroupFooterSummaryType != SummaryItemType.None) {
                         IDxGridSummaryItemsOwner summaryItemsOwner = (IDxGridSummaryItemsOwner)dataGridAdapter;
                         var summaryItem = (DxGridSummaryItemWrapper)summaryItemsOwner.CreateItem(modelColumn.Id, modelColumnExtender.GroupFooterSummaryType);
                         summaryItem.SummaryItemModel.FooterColumnName = modelColumn.Id;
                         summaryItemsOwner.GroupSummary.Add(summaryItem);
                     }
                 }
             }
         }
     }
}

```

## Ejecutar la aplicación

Vuelva a generar la solución. Invoque el  **Editor de modelos**  para el módulo base y desplácese hasta un nodo  **ListView**. Cambie la siguiente configuración:

-   Establezca la propiedad en para permitir a los usuarios agrupar datos. `IsGroupPanelVisible``true`
-   Establezca la nueva propiedad en para habilitar los pies de página del grupo. `IsGroupFooterVisible``true`
-   Especifique los tipos de resumen para los nodos  **de columna**. Utilice la nueva propiedad.`GroupFooterSummaryType`

Ejecute la aplicación. Abra una vista de lista con un panel de grupo y agrupe la vista de lista por una columna. Puede ver el pie de página con los tipos de resumen especificados.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/028a605d-1113-4342-9399-a6a471f82542)


# Agregar nodos y propiedades personalizados al modelo de aplicación en código


En este tema se describe cómo extender el modelo de  [aplicación](https://docs.devexpress.com/eXpressAppFramework/112579/ui-construction/application-model-ui-settings-storage)  generado automáticamente con nodos y propiedades personalizados en el código. Utilice el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor)  para editar visualmente el modelo de aplicación.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/00c8e14c-1665-474f-b93d-c9d31484ec8c)

>PROPINA
Si necesita cambiar las propiedades de nodo existentes, vea el tema siguiente: [Leer y establecer valores para nodos integrados del modelo de aplicación en código](https://docs.devexpress.com/eXpressAppFramework/112810/ui-construction/application-model-ui-settings-storage/customize-application-model-in-code/access-the-application-model-in-code).

## Agregar una propiedad personalizada a un nodo existente

En este ejemplo se muestra cómo agregar  _MyCustomProperty_  al nodo Modelo raíz.

1.  Declare una interfaz descendiente  [de IModelNode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelNode)  con una propiedad personalizada.
    
    **Archivo:**  _MySolution.Module\Module.cs(vb)_
    

    
    ```csharp
    using DevExpress.ExpressApp.Model;
    // ...
    namespace MySolution.Module {
        // ... 
        public interface IModelMyModelExtension : IModelNode {
            string MyCustomProperty { get; set; }
        }
    }    
    
    ```
    
2.  Reemplace el método  [ModuleBase.ExtendModelInterfaces](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ModuleBase.ExtendModelInterfaces(DevExpress.ExpressApp.Model.ModelInterfaceExtenders))  para agregar la propiedad personalizada al nodo:
    
    **Archivo:**  _MySolution.Module\Module.cs(vb)_
    

    
    ```csharp
    using DevExpress.ExpressApp.Model;
    // ...
    namespace MySolution.Module {
        public sealed partial class MySolutionModule : ModuleBase {
            // ...
            public override void ExtendModelInterfaces(ModelInterfaceExtenders extenders) {
                base.ExtendModelInterfaces(extenders);
                extenders.Add<IModelApplication, IModelMyModelExtension>();
            }
            // ...
        }
    }
    
    ```
    
    Como alternativa,  [implemente la interfaz IModelExtender](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.IModelExtender)  en un controlador y reemplace el método  [ModuleBase.ExtendModelInterfaces](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ModuleBase.ExtendModelInterfaces(DevExpress.ExpressApp.Model.ModelInterfaceExtenders))  en el código del  [controlador](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers):
    

    
    ```csharp
    using DevExpress.ExpressApp.Model;
    using System.ComponentModel;
    // ...
    namespace MySolution.Module {
    
        public class MyController : ViewController<ListView>, IModelExtender {
        // ...
            public override void ExtendModelInterfaces(ModelInterfaceExtenders extenders) {
                base.ExtendModelInterfaces(extenders);
                extenders.Add<IModelApplication, IModelMyModelExtension>();
            }
        }
    }
    
    ```
    
3.  Vuelva a generar la solución y abra el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor)  para comprobar el resultado. El nodo  [raíz IModelApplication](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelApplication)  contiene  **MyCustomProperty**.
    
![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/8852957a-f85a-46ec-95e2-d6330d503623)
    

Para agregar  **MyCustomProperty**  a otro nodo, pase la interfaz correspondiente a este nodo en lugar de  **IModelApplication**  a los  **extensores. Agregar**  método. En el tema siguiente se enumeran las interfaces disponibles:  [Interfaces de modelo de aplicación suministradas con XAF.](https://docs.devexpress.com/eXpressAppFramework/403535/ui-construction/application-model-ui-settings-storage/how-application-model-works/application-model-interfaces)

En XAF, el nombre de una propiedad personalizada no puede ser  _Application_. La interfaz  [IModelNode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelNode)  ya incluye la propiedad  [Application](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelNode.Application)  que devuelve el nodo raíz del modelo de aplicación. Si implementa una propiedad personalizada denominada  _Application_, se produce la siguiente excepción:  _No se puede compilar el código generado_.

Puede decorar las propiedades del nodo con los siguientes atributos:

[`Browsable`](https://docs.microsoft.com/dotnet/api/system.componentmodel.browsableattribute)

Especifica si se muestra una propiedad en el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor). Si utiliza la técnica descrita anteriormente para agregar una propiedad, el Editor de modelos muestra la propiedad. Para ocultar esta propiedad, decórela con el atributo  **Browsable**  y pase  **false**  como parámetro.

[`Category`](https://docs.microsoft.com/dotnet/api/system.componentmodel.categoryattribute)

Especifica una categoría. El Editor de modelos agrupa las propiedades de cada nodo por sus categorías en la cuadrícula de propiedades. La categoría predeterminada es  _Misc_. Para asignar una categoría a una propiedad, decore la propiedad con el atributo  **Category**  y pase un nombre de categoría como parámetro. Si la categoría especificada no existe, XAF agrega esta categoría.

[`DataSourceProperty`](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.DataSourcePropertyAttribute)

Especifica un nombre de una propiedad que contiene una lista de valores predefinidos para la propiedad actual. XAF genera una lista desplegable en la cuadrícula de propiedades del Editor de modelos para mostrar estos valores. Si el nodo actual expone una lista de nodos secundarios (implementa la interfaz  **IModelList<**_ChildNodeType_**>**), puede  _pasarla_  como el parámetro de atributo  **DataSourceProperty**  para mostrar los nodos secundarios como valores predefinidos en la cuadrícula de propiedades.

[`DefaultValue`](https://docs.microsoft.com/dotnet/api/system.componentmodel.defaultvalueattribute)

Especifica el valor predeterminado de una propiedad.

[`Editor`](https://docs.microsoft.com/dotnet/api/system.componentmodel.editorattribute)

Enlaza un editor personalizado a la propiedad.

[`Description`](https://docs.microsoft.com/dotnet/api/system.componentmodel.descriptionattribute)

Especifica la descripción de una propiedad que se muestra en la parte inferior del Editor de modelos.

[`Localizable`](https://docs.microsoft.com/dotnet/api/system.componentmodel.localizableattribute)

Especifica si una propiedad se puede  [localizar](https://docs.devexpress.com/eXpressAppFramework/112595/localization/localization-basics). Para definir una propiedad localizable, decórela con el atributo  **Localizable**  **y pase true**  como parámetro.

[`ReadOnly`](https://docs.microsoft.com/dotnet/api/system.componentmodel.readonlyattribute)

Especifica si un usuario puede cambiar el valor de la propiedad en el Editor de modelos. Decore una propiedad con el atributo  **ReadOnly**  **y pase true**  como parámetro para prohibir la modificación de valores en el Editor de modelos. Como alternativa, omita el descriptor de acceso establecido.

[`Required`](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.requiredattribute)

Especifica si se requiere una propiedad. Al decorar una propiedad con el atributo  **Required**, el Editor de modelos no permite guardar los cambios si no se establece el valor de la propiedad.

El código siguiente agrega el atributo  **Localizable**  a  **MyCustomProperty y hace que MyCustomRequiredProperty**  sea necesario:

**Archivo:**  _MySolution.Module\Module.cs(vb)_



```csharp
using DevExpress.ExpressApp.Model;
using System.ComponentModel;
// ...
namespace MySolution.Module {
    // ...
    public interface IModelMyModelExtension : IModelNode {
        [Localizable(true)]
        string MyCustomProperty { get; set; }
        [Required]
        string MyCustomRequiredProperty { get; set; }
    }
}   

```

## Agregar un nuevo nodo al modelo de aplicación

Para crear un nodo personalizado, declare una interfaz descendiente  [de IModelNode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelNode)  con propiedades personalizadas.

Si una propiedad de interfaz heredada de  **IModelNode**  omite el descriptor de acceso establecido, el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor)  lo muestra como un nodo (**MyChildNode**  en el ejemplo siguiente). Si una propiedad de interfaz tiene el descriptor de acceso establecido, el Editor de modelos lo muestra como una propiedad en la cuadrícula de propiedades (**MyProperty**  en el ejemplo siguiente).



```csharp
[KeyProperty(nameof(Name))]
public interface IModelMyChildNode : IModelNode {
    string Name { get; set; }
    // ...
}

public interface IModelMyNodeWithChildNode : IModelNode {
    IModelMyChildNode MyChildNode { get; }
    IModelMyChildNode MyProperty { get; set; }
}

```

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/5ad5614f-b36a-4fef-95ed-e25e6fff0e38)

En el ejemplo siguiente se definen las interfaces IModelMyChildNode e  **IModelMyNodeWithChildNode derivadas de IModelNode**.[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelNode)

**Archivo:**  _MySolution.Module\Module.cs(vb)_



```csharp
using System.ComponentModel;
// ...
namespace MySolution.Module {
    // ...
    [KeyProperty(nameof(Name))]
    public interface IModelMyChildNode : IModelNode {
        string Name { get; set; }
        [Localizable(true)]
        string MyStringProperty { get; set; }
        int MyIntegerProperty { get; set; }
    }
    public interface IModelMyNodeWithChildNode : IModelNode {
        IModelMyChildNode MyChildNode { get; }
    }
    public interface IModelMyNodeWithChildNodes : IModelNode, IModelList<IModelMyChildNode> {
    }
}

```

**IModelMyNodeWithChildNodes**  implementa la interfaz IModelList<IModelMyChildNode> para exponer la lista de nodos  **IModelMyChildNode**.

Puede decorar interfaces de nodo con los siguientes atributos:

**Descripción**

Especifica una descripción de nodo que se muestra en la parte inferior del Editor de modelos.

**DisplayName**

Especifica un título de nodo en el Editor de modelos.

**DisplayProperty**

Especifica el nombre de propiedad del nodo, cuyo valor se utiliza como título del nodo en el Editor de modelos.

[`ImageName`](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.ImageNameAttribute)

Especifica una imagen de nodo que el Editor de modelos muestra para el nodo.

**KeyProperty**

Especifica una propiedad de clave de nodo. XAF utiliza propiedades clave para identificar nodos. Una propiedad key debe ser del tipo string. Si no utiliza el atributo  **KeyProperty**, XAF genera la propiedad  **Id**. En el ejemplo anterior,  _Name_  es la propiedad key de  _IModelMyChildNode_.

Para agregar los nodos  **ModelMyNodeWithChildNodes**  e  **IModelMyNodeWithChildNode**  al modelo de aplicación, extienda la interfaz de nodo primario correspondiente como se describe en la sección anterior:  [Agregar una propiedad personalizada al nodo existente](https://docs.devexpress.com/eXpressAppFramework/404125/ui-construction/application-model-ui-settings-storage/modify-application-model/add-nodes-and-node-properties-to-app-model#add-a-custom-property-to-an-existing-node).

**Archivo:**  _MySolution.Module\Module.cs(vb)_



```csharp
using System.ComponentModel;

namespace MySolution.Module {
    public sealed partial class MySolutionModule : ModuleBase {    
        // ...
        public override void ExtendModelInterfaces(ModelInterfaceExtenders extenders) {
            base.ExtendModelInterfaces(extenders);
            extenders.Add<IModelApplication, IModelMyModelExtension>();
        }
    }

    public interface IModelMyModelExtension : IModelNode {
        // ...
        IModelMyNodeWithChildNode MyNodeWithOneChildNode { get; }
        IModelMyNodeWithChildNodes MyNodeWithSeveralChildNodes { get; }
    }
}

```

Vuelva a generar la solución y abra el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor)  para comprobar el resultado.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/33a555c1-829f-4bfe-9814-655a529c5fc9)

Puede utilizar el menú contextual para agregar nodos secundarios al nodo  **MyNodeWithSeveralChildNodes**.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/35b13694-b6c7-4428-a0c8-a6a64e514f3b)

## Usar un generador de nodos para agregar varios nodos secundarios

Si necesita agregar un lote de nodos secundarios similares a un nodo principal, utilice un generador de nodos. Cree un descendiente  [de ModelNodesGeneratorBase](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.ModelNodesGeneratorBase)  y reemplace el método virtual  **GenerateNodesCore**. Utilice el parámetro  _node_  para acceder al nodo principal. El generador de nodos siguiente crea diez nodos  **IModelMyChildNode**  y les asigna índices:

**Archivo:**  _MySolution.Module\Module.cs(vb)_



```csharp
using DevExpress.ExpressApp.Model.Core;
// ...
namespace MySolution.Module {
    // ...
    public class MyChildNodeGenerator : ModelNodesGeneratorBase {
        protected override void GenerateNodesCore(ModelNode node) {
            for (int i = 0; i < 10; i++) {
                string childNodeName = "MyChildNode " + i.ToString();
                node.AddNode<IModelMyChildNode>(childNodeName);
                node.GetNode(childNodeName).Index = i;
            }
        }
    }
}

```

>PROPINA
Puede utilizar la propiedad para tener acceso al nodo raíz del modelo de aplicación en el código del método  **Generarnodosprincipales**.`node.Application`

Utilice  [ModelNodesGeneratorAttribute](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.ModelNodesGeneratorAttribute)  para asociar una implementación de nodo con un generador de nodos:

**Archivo:**  _MySolution.Module\Module.cs(vb)_



```csharp
namespace MySolution.Module {
    // ...
    [ModelNodesGenerator(typeof(MyChildNodeGenerator))]
    public interface IModelMyNodeWithChildNodes : IModelNode, IModelList<IModelMyChildNode> {
    }
    // ...
}

```

Vuelva a generar la solución y abra el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor)  para ver los cambios.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/e054c0b5-7525-44ce-8f1f-805ae3ea1fdb)

Los generadores de nodos realizan cambios en  [la capa cero del modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works). XAF ejecuta el método  **GenerateNodesCore**  cuando se solicitan los datos  **MyNodeWithSeveralChildNodes**  por primera vez y almacena estos datos en el modelo de aplicación.

Puede implementar un único generador de nodos para cada nodo. Utilice  [Generator Updaters](https://docs.devexpress.com/eXpressAppFramework/404125/ui-construction/application-model-ui-settings-storage/modify-application-model/add-nodes-and-node-properties-to-app-model#use-generator-updaters-to-customize-nodes)  para personalizar nodos adicionales.

## Implementar una propiedad con una lista de valores predefinidos

Utilice el atributo  **DataSourceProperty**  para agregar una lista de valores predefinidos a una propiedad de nodo. Pase el nombre de una propiedad de nodo que contenga una lista de valores como parámetro  **DataSourceProperty**. También puede utilizar  _esto_  para mostrar los nodos secundarios del nodo actual en la lista desplegable con valores disponibles.

### Mostrar nodos secundarios en la lista desplegable

En el ejemplo siguiente se extiende el nodo  **IModelMyNodeWithChildNodes**  con la propiedad  **SelectedChildNode**. Esta propiedad permite a los usuarios seleccionar nodos secundarios como valores disponibles.

**Archivo:**  _MySolution.Module\Module.cs(vb)_



```csharp
using DevExpress.Persistent.Base;
// ...
namespace MySolution.Module {
    // ...
    public interface IModelMyNodeWithChildNodes : IModelNode, IModelList<IModelMyChildNode> {
        [DataSourceProperty("this")]
        IModelMyChildNode SelectedChildNode { get; set; }
    }
}

```

Vuelva a generar la solución para actualizar el modelo de aplicación. La siguiente imagen muestra el resultado.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/17296624-8ffc-454f-99cc-da804751cc94)

Si agrega o quita nodos secundarios, la lista desplegable refleja los cambios inmediatamente.

### Mostrar valores personalizados en la lista desplegable

En el ejemplo siguiente se implementa la propiedad  **ListProperty**  personalizada y se muestra una lista de valores predefinidos para esta propiedad en la cuadrícula de propiedades. Para definir una lista de valores, siga estos pasos:

1.  Implemente una propiedad de colección sin el descriptor de acceso set (**SourceListProperty**  en el ejemplo siguiente). Aplique el atributo  **Browsable(false)**  a esta propiedad para ocultarla de la estructura del modelo de aplicación.
    
2.  Decore  **ListProperty**  con el atributo  **DataSourceProperty**  y utilice el nombre de la propiedad de colección como parámetro de atributo.
    
3.  Cree una clase estática y deformarla con el atributo  **DomainLogic**. Utilice la interfaz de nodo, que contiene la propiedad  **ListProperty**, como parámetro de atributo.
    
4.  Agregue el método  **Get_SourceListProperty**  a esta clase. El método  **Get_SourceListProperty**  devuelve el valor  **SourceListProperty**. Este valor contiene una lista de valores predefinidos para  **ListProperty**. En el ejemplo, devuelve los siguientes valores: .`"Value1", "value2", "Value3"`
    

**Archivo:**  _MySolution.Module\Module.cs(vb)_



```csharp
public interface IModelMyNodeWithChildNodes : IModelNode, IModelList<IModelMyChildNode> {
    [DataSourceProperty("SourceListProperty")]
    String ListProperty { get; set; }

    [Browsable(false)]
    IEnumerable<String> SourceListProperty {
        get;
    }
}
[DomainLogic(typeof(IModelMyNodeWithChildNodes))]
public static class ModelMyNodeWithChildNodesLogic {
    public static IEnumerable<string> Get_SourceListProperty(IModelMyNodeWithChildNodes myNodeWithChildNodes) {
        return new List<string>() { "Value1", "value2", "Value3" };
    }
}

```

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/807832ce-5d95-41bf-8d7c-907c8844e0ec)

## Usar actualizadores de generadores para personalizar nodos

Utilice Generator Updaters para actualizar en masa los nodos secundarios. Cree una clase Generator Updater como descendiente de  [ModelNodesGeneratorUpdater<T>](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.ModelNodesGeneratorUpdater-1). La clase  **ModelNodesGeneratorUpdater<T>**  expone el método virtual  [UpdateNode(ModelNode).](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelNodesGeneratorUpdater.UpdateNode(DevExpress.ExpressApp.Model.Core.ModelNode))  Reemplace este método para actualizar los nodos secundarios. Utilice el parámetro  _node_  para acceder al nodo principal.

Puede agregar varios actualizadores de generadores a un generador de nodos. El ejemplo siguiente implementa dos actualizadores de generadores. El primer Updater establece el valor  **IModelMyChildNode.MyIntegerProperty**  para cada nodo, el segundo establece la propiedad  **IModelMyNodeWithChildNodes.SelectedChildNode.**



```csharp
namespace MySolution.Module {
    // ...
    public class MyChildNodesUpdater1 : ModelNodesGeneratorUpdater<MyChildNodeGenerator> {
        public override void UpdateNode(ModelNode node) {
            foreach (IModelMyChildNode childNode in ((IModelMyNodeWithChildNodes)node)) {
                if (childNode.Index.HasValue) {
                        childNode.MyIntegerProperty = (int)childNode.Index + 1;
                }
            }
        }
    }
    public class MyChildNodesUpdater2 : ModelNodesGeneratorUpdater<MyChildNodeGenerator> {
        public override void UpdateNode(ModelNode node) {
            ((IModelMyNodeWithChildNodes)node).SelectedChildNode =
                ((IModelMyNodeWithChildNodes)node)["MyChildNode 9"];
        }
    }
}

```

>PROPINA
Puede utilizar la propiedad para tener acceso al nodo raíz Application Model en el código del método  **UpdateNode**.`node.Application`

Utilice el método  [ModuleBase.AddGeneratorUpdaters para registrar Generator Updaters.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ModuleBase.AddGeneratorUpdaters(DevExpress.ExpressApp.Model.Core.ModelNodesGeneratorUpdaters))

**Archivo:**  _MySolution.Module\Module.cs(vb)_



```csharp
using DevExpress.ExpressApp.Model.Core;
// ...
public sealed partial class MySolutionModule : ModuleBase {
    // ...       
    public override void AddGeneratorUpdaters(ModelNodesGeneratorUpdaters updaters) {
        base.AddGeneratorUpdaters(updaters);
        updaters.Add(new MyChildNodesUpdater1());
        updaters.Add(new MyChildNodesUpdater2());
    }
    // ...
}

```

Vuelva a generar la solución y abra el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor)  para ver el resultado.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/bc1c04f0-705d-4331-b778-4bce55c134b4)

Es posible que no vea los cambios como se muestra en la imagen de arriba. Esto sucede si modifica estos valores de propiedad en el Editor de modelos antes de ejecutar el código. Los actualizadores del generador funcionan en la  [capa](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)  cero del modelo de aplicación y los cambios realizados en tiempo de diseño anulan la configuración del actualizador. Restablezca las diferencias para el nodo  **MyNodeWithSeveralChildNodes**  para ver los cambios: haga clic con el botón secundario en el nodo y seleccione  **Restablecer diferencias**.

>NOTA
Los generadores de nodos  y los actualizadores de nodos funcionan en la capa cero del modelo de aplicación. El almacenamiento de  [diferencias de modelo de usuario](https://docs.devexpress.com/eXpressAppFramework/403527/ui-construction/application-model-ui-settings-storage/change-application-model) no contiene estos cambios. Para personalizar nodos en la capa Diferencias del modelo de usuario, utilice las técnicas del tema siguiente: [Cómo cambiar el valor predeterminado del modelo de aplicación globalmente o para varios nodos](https://supportcenter.devexpress.com/Ticket/Details/T271022/how-to-change-the-default-application-model-value-globally-or-for-multiple-nodes).

Puede utilizar la técnica descrita anteriormente para conectar actualizadores de generadores a  [generadores de nodos integrados](https://docs.devexpress.com/eXpressAppFramework/113316/ui-construction/application-model-ui-settings-storage/how-application-model-works/built-in-nodes-generators). Los siguientes temas contienen ejemplos de generadores integrados:

-   [EnumDescriptor.GenerateDefaultCaptions](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Utils.EnumDescriptor.GenerateDefaultCaptions.overloads)
-   [Nodo modelo de aplicación de filtros](https://docs.devexpress.com/eXpressAppFramework/112992/filtering/in-list-view/filters-application-model-node)
-   [Implementar un elemento de vista](https://docs.devexpress.com/eXpressAppFramework/112641/ui-construction/view-items-and-property-editors/implement-a-view-item)
-   [Crear nodos ListView adicionales en código mediante un actualizador de generador](https://docs.devexpress.com/eXpressAppFramework/113315/ui-construction/application-model-ui-settings-storage/customize-application-model-in-code/create-additional-list-view-nodes-in-code-using-a-generator-updater)


# Crear nodos de vista de lista adicionales en código mediante un actualizador degenerador


De forma predeterminada, se generan dos nodos ListView en el  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112579/ui-construction/application-model-ui-settings-storage)  para cada clase empresarial. Estos nodos representan una vista de lista de uso general y una vista de lista de búsqueda que contiene menos columnas (consulte  [Generación de columnas de vista de lista](https://docs.devexpress.com/eXpressAppFramework/113285/ui-construction/views/layout/list-view-column-generation)). A menudo, se requiere agregar más vistas de lista manualmente. Estas vistas de lista adicionales se pueden utilizar como  [variantes de vista](https://docs.devexpress.com/eXpressAppFramework/113011/application-shell-and-base-infrastructure/view-variants-module), elementos de  [panel](https://docs.devexpress.com/eXpressAppFramework/113296/ui-construction/views/layout/display-several-views-side-by-side), etc. Normalmente, esta tarea se puede realizar en el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor). Sin embargo, en ciertos escenarios, puede ser necesario agregar nodos en el código.

El proceso predeterminado de generación de los nodos secundarios del nodo  **Views**  se gestiona mediante el generador de nodos  [ModelViewsNodesGenerator](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.NodeGenerators.ModelViewsNodesGenerator)  integrado. Para personalizar este proceso, debe "adjuntar" una clase Generator Updater a este Generator. En este tema se describe cómo implementar un actualizador de generador que crea nodos ListView personalizados. Además, se ilustra un actualizador que crea variantes de vista utilizando estos nuevos nodos. Para obtener más información sobre los generadores de nodos y los actualizadores de generadores, consulte el tema  [Extender y personalizar el modelo de aplicación en código](https://docs.devexpress.com/eXpressAppFramework/112810/ui-construction/application-model-ui-settings-storage/customize-application-model-in-code/access-the-application-model-in-code).

## Crear vistas de lista

Consideremos la siguiente clase de negocio  **de empleados**.

**EF Core**

```csharp
[DefaultClassOptions,ImageName("BO_Person")]
public class Employee : BaseObject {
    public virtual string FirstName { get; set; }
    public virtual string LastName { get; set; }
    public string FullName {
        get { return String.Format("{0} {1}", FirstName, LastName); }
    }
    public virtual string Position { get; set; }
    public virtual string Email { get; set; }
}

// Make sure that you use options.UseChangeTrackingProxies() in your DbContext settings.

```

**XPO**

```csharp
[DefaultClassOptions, ImageName("BO_Person")]
public class Employee : BaseObject {
    public Employee(Session session) : base(session) { }
    private string firstName;
    private string lastName;
    private string position;
    private string email;
    public string FirstName {
        get { return firstName; }
        set { SetPropertyValue(nameof(FirstName), ref firstName, value); }
    }
    public string LastName {
        get { return lastName; }
        set { SetPropertyValue(nameof(LastName), ref lastName, value); }
    }
    public string FullName {
        get { return String.Format("{0} {1}", FirstName, LastName); }
    }
    public string Position {
        get { return position; }
        set { SetPropertyValue(nameof(Position), ref position, value); }
    }
    public string Email {
        get { return email; }
        set { SetPropertyValue(nameof(Email), ref email, value); }
    }
}
```
A continuación se ilustra la vista de lista generada de forma predeterminada para esta clase.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/034461be-56c3-4c3e-a3eb-eb4aefaee809)

Esta vista de lista está definida por  **Vistas**  |  **Employee_ListView**  nodo. Vamos a implementar un Generador Actualizador que genera dos vistas de lista adicionales, que son:

1.  Una vista de lista con columnas ocultas  **Nombre**  y  **Apellido**  (**Employee_ListView_FewColumns**);
2.  Una vista de lista donde los registros se agrupan por la columna  **Posición**  (**Employee_ListView_Grouped**).

Una clase Generator Updater debe heredar la clase abstracta  [ModelNodesGeneratorUpdater<T>](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.ModelNodesGeneratorUpdater-1), donde  **T**  es el tipo del generador de nodos que se va a personalizar. Un generador de nodos integrado que genera el contenido del nodo  **Vistas**  es  **ModelViewsNodesGenerator**. El método abstracto  [ModelNodesGeneratorUpdater'1.UpdateNode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.ModelNodesGeneratorUpdater-1.UpdateNode(DevExpress.ExpressApp.Model.Core.ModelNode))  debe implementarse en un actualizador de generador personalizado. El parámetro del tipo  **ModelNode**, pasado a este método, representa un nodo generado. En nuestro caso, este nodo es el nodo Vistas. Para agregar nodos secundarios, se puede usar el método  **ModelNode.AddNode<T>.**  Tenga en cuenta que la propiedad  [IModelObjectView.ModelClass](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelObjectView.ModelClass)  debe especificarse para cada nodo  **ListView**. Después de eso, se puede acceder a las propiedades del nodo  **ListView**  y sus valores se pueden personalizar. Para tener acceso al nodo  **Columns**, utilice la propiedad  [IModelListView.Columns.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelListView.Columns)  El siguiente fragmento de código ilustra la clase  **AddListViewNodesGeneratorUpdater Generator Updater**.



```csharp
using DevExpress.ExpressApp.Model;
using DevExpress.ExpressApp.Model.Core;
using DevExpress.ExpressApp.Model.NodeGenerators;
// ...
public class AddListViewNodesGeneratorUpdater : 
    ModelNodesGeneratorUpdater<ModelViewsNodesGenerator> {
    public const string FewColumnsListViewNodeIdSuffix = "_FewColumns";
    string[] columnsToHideIds = { "FirstName", "LastName" };
    public const string GroupedListViewNodeIdSuffix = "_Grouped";
    public const string GroupByColumnId = "Position";
    static Type targetType = typeof(Employee);
    public override void UpdateNode(ModelNode viewsNode) {
        AddFewColumnsListViewNode(viewsNode);
        AddGroupedListViewNode(viewsNode);
    }
    public static IModelListView GetDefaultListView(ModelNode viewsNode) {
        return viewsNode.Application.BOModel.GetClass(targetType).DefaultListView;
    }
    IModelListView AddListViewNode(ModelNode viewsNode, string listViewId) {
        IModelListView listViewNode = (IModelListView)viewsNode.AddNode<IModelListView>(listViewId);
        listViewNode.ModelClass = viewsNode.Application.BOModel.GetClass(targetType);
        return listViewNode;
    }
    void AddFewColumnsListViewNode(ModelNode viewsNode) {
        string nodeId = GetDefaultListView(viewsNode).Id + FewColumnsListViewNodeIdSuffix;
        IModelListView fewColumnsListViewNode = AddListViewNode(viewsNode, nodeId);
        IModelColumns columns = (IModelColumns)fewColumnsListViewNode.Columns;
        foreach (string columnId in columnsToHideIds) {
            columns[columnId].Index = -1;
        }
    }
    void AddGroupedListViewNode(ModelNode viewsNode) {
        string nodeId = GetDefaultListView(viewsNode).Id + GroupedListViewNodeIdSuffix;
        IModelListView groupedListViewNode =
            AddListViewNode(viewsNode, nodeId);
        IModelColumns columns = (IModelColumns)groupedListViewNode.Columns;
        columns[GroupByColumnId].GroupIndex = 1;
    }
}

```

Puede colocar esta clase en un archivo de código independiente en el  [proyecto de módulo](https://docs.devexpress.com/eXpressAppFramework/118045/application-shell-and-base-infrastructure/application-solution-components/application-solution-structure)  o agregarla al archivo Module_.cs_  (_Module.vb_). El actualizador del generador implementado debe registrarse en el método  [ModuleBase.AddGeneratorUpdaters](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ModuleBase.AddGeneratorUpdaters(DevExpress.ExpressApp.Model.Core.ModelNodesGeneratorUpdaters))  reemplazado de la siguiente manera:


```csharp
using DevExpress.ExpressApp.Model.Core;
// ...
public sealed partial class CreateNodesInCodeModule : ModuleBase {
    // ...
    public override void AddGeneratorUpdaters(ModelNodesGeneratorUpdaters updaters) {
        base.AddGeneratorUpdaters(updaters);
        updaters.Add(new AddListViewNodesGeneratorUpdater());
    }
}

```

Después de implementar y registrar Generator Updater, vuelva a generar la solución e invoque el Editor de modelos. Los nuevos nodos ListView se ilustran a continuación.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/b1b1c705-d7db-4bcb-b5af-c91a15130c52)

>NOTA
Los nodos y propiedades personalizados no están marcados en negrita en el Editor de modelos, ya que estos cambios se generan en el código. Puede consultar el _modelo.Diferencias diseñadas.  Origen XAFML_ para asegurarse de que no contiene personalizaciones relacionadas con los nodos generados.

## Crear variantes de vista

Vamos a implementar variantes de vista para la vista de lista de empleados para que las vistas de lista creadas en la sección anterior de este tema sean visibles en la interfaz de usuario. Aunque las variantes de vista se diseñan normalmente en el editor de modelos, también es posible crear variantes de vista en código, a través del actualizador del generador. El actualizador del generador que genera variantes de vista mediante vistas creadas por  **AddListViewNodesGeneratorUpdater**  se ilustra en el fragmento de código siguiente. El módulo View Variants debe agregarse antes de implementar esta clase.



```csharp
using DevExpress.ExpressApp.Model;
using DevExpress.ExpressApp.Model.Core;
using DevExpress.ExpressApp.Model.NodeGenerators;
using DevExpress.ExpressApp.ViewVariantsModule;
// ...
class AddViewVariantsGeneratorUpdater : ModelNodesGeneratorUpdater<ModelViewsNodesGenerator> {
    public override void UpdateNode(ModelNode viewsNode) {
        IModelView rootView = AddListViewNodesGeneratorUpdater.GetDefaultListView(viewsNode);
        IModelView fewColumnsListView = 
            (IModelView)viewsNode.GetNode(rootView.Id +
            AddListViewNodesGeneratorUpdater.FewColumnsListViewNodeIdSuffix);
        IModelView groupedListView =
            (IModelView)viewsNode.GetNode(rootView.Id +
            AddListViewNodesGeneratorUpdater.GroupedListViewNodeIdSuffix);
        AddVariant("Default", "Default", rootView, rootView, true);
        AddVariant("FewColumns", "Few Columns", rootView, fewColumnsListView, false);
        AddVariant("Grouped", "Grouped", rootView, groupedListView, false);
    }
    void AddVariant(string variantId, string caption, 
        IModelView rootView, IModelView variantView, bool isCurrent) {
        IModelVariants variants = ((IModelViewVariants)rootView).Variants;
        IModelVariant variant = variants.AddNode<IModelVariant>(variantId);
        variant.View = variantView;
        variant.Caption = caption;
        if (isCurrent) variants.Current = variant;
    }
}

```

Este actualizador del generador también debe registrarse en el método  **AddGeneratorUpdaters**  del módulo.



```csharp
public sealed partial class CreateNodesInCodeModule : ModuleBase {
    // ...
    public override void AddGeneratorUpdaters(ModelNodesGeneratorUpdaters updaters) {
        base.AddGeneratorUpdaters(updaters);
        updaters.Add(new AddListViewNodesGeneratorUpdater());
        updaters.Add(new AddViewVariantsGeneratorUpdater());
    }
}

```

Después de implementar y registrar Generator Updater, vuelva a generar la solución e invoque el Editor de modelos para el proyecto de módulo. Los nuevos nodos Variant se ilustran a continuación.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/6efe61f7-c9ed-438d-9a53-88b8306ec2d8)

Ahora puede probar las vistas implementadas y las variantes de vista en tiempo de ejecución.

**Aplicación de Windows Forms**

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/94b0029f-e558-48e8-aee0-b8470e4bcdf7)

**ASP.NET Aplicación de formularios Web Forms**

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/948bc11a-5dfe-4f97-897f-81c794fb7a4f)

**Aplicación Blazor**

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/469a8d75-853a-448e-8bd3-4c51927274b5)

Puede utilizar un enfoque similar para crear actualizadores de generadores para generadores de nodos integrados o personalizados.


# Aplicar inmediatamente los cambios del modelo de aplicación a la vista actual

En el ejemplo de este tema se muestra cómo actualizar el  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112579/ui-construction/application-model-ui-settings-storage)  y aplicar los cambios a una vista inmediatamente. La aplicación no vuelve a cargar la vista si sigue las instrucciones que se indican a continuación.

El código siguiente implementa dos  [acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  que ilustran este enfoque para la personalización del modelo.



```csharp
using System;
using DevExpress.ExpressApp;
using DevExpress.ExpressApp.Actions;
using DevExpress.ExpressApp.Scheduler.Win;
using DevExpress.ExpressApp.Win.Editors;
using DevExpress.Persistent.Base;
using DevExpress.Persistent.Base.General;
// ...
public class RefreshViewControlsAfterModelChangesViewController :
        ObjectViewController<ListView, IEvent> {
    public RefreshViewControlsAfterModelChangesViewController() {
        new SimpleAction(this, "SwitchMasterDetailMode", 
         PredefinedCategory.View.ToString(), (s, e) => {

            // Obtain and save the view
            ListView savedView = (ListView)Frame.View;

            // Detach the View from the Frame 
            // Don't dispose the old view
            if(Frame.SetView(view: null, true, null, disposeOldView: false)) {

                // Change the Application Model
                MasterDetailMode defaultMasterDetailMode = MasterDetailMode.ListViewOnly;
                savedView.Model.MasterDetailMode = 
                    savedView.Model.MasterDetailMode == defaultMasterDetailMode ?
                    MasterDetailMode.ListViewAndDetailView : defaultMasterDetailMode;

                // Load Model changes into the View 
                savedView.LoadModel(false);

                // Re-attach the View back to its Frame
                Frame.SetView(savedView);
            }
        });
        new SimpleAction(this, "SwitchEditor", 
         PredefinedCategory.View.ToString(), (s, e) => {
            // Same algorithm as above
            var savedView = View;
            if(Frame.SetView(view: null, true, null, disposeOldView: false)) {
                Type defaultListEditorType = Application.Model.Views.DefaultListEditor;
                savedView.Model.EditorType = 
                    savedView.Model.EditorType == defaultListEditorType ? 
                    typeof(SchedulerListEditor) : defaultListEditorType;
                savedView.LoadModel(false);
                Frame.SetView(savedView);
            }
        });
    }
}

```

>CAUTELA
No diseñamos esta solución para ser utilizada en el [Controller.Controlador](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Controller.Activated) de eventos activado o en un DevExpress.ExpressApp.Controller.  Anulación del método activado. En estos casos, el uso de la solución puede dar lugar a resultados inesperados.


# Cambiar la interfaz de usuario de View y omitir actualizaciones del modelo de aplicación


En este tema se explica cómo cambiar la interfaz de usuario de View de tal manera que las modificaciones no persistan entre ejecuciones de aplicaciones.

Puede cambiar la configuración de Vista en el código. Los usuarios pueden reorganizar los elementos mientras usan la aplicación. XAF guarda todos estos cambios como  [diferencias de modelo](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works). En cada inicio de la aplicación, carga estas diferencias para que los usuarios puedan continuar con la misma interfaz de usuario que vieron durante la última ejecución de la aplicación.

Puede forzar a XAF a no guardar las  **diferencias**  de modelo: aplique los cambios de una manera que omita el  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112579/ui-construction/application-model-ui-settings-storage). A continuación, la aplicación revertirá los cambios en los elementos de la interfaz de usuario en el próximo inicio.

## Aplicar cambios directamente a los controles de interfaz de usuario

El código siguiente muestra cómo modificar la interfaz de usuario y mantener el  **modelo de aplicación**  sin cambios. Acceda directamente a las propiedades de los controles de interfaz de usuario subyacentes. No utilice la configuración  **del modelo de aplicación**  (como  [ViewItem.Caption](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Editors.ViewItem.Caption)).


```csharp
using DevExpress.ExpressApp;
using YourSolutionName.Module.BusinessObjects;
using DevExpress.ExpressApp.Win.Editors;
using DevExpress.ExpressApp.Editors;
using DevExpress.XtraEditors;

namespace YourSolutionName.Module.Win.Controllers {
    public class CustomWinController : ObjectViewController<DetailView,Contact> {    
        protected override void OnActivated() {
            base.OnActivated();
            View.CustomizeViewItemControl(this, CustomizeViewItem_Direct, "myStaticText1");
        }

        // This code saves the changes to the Application Model.
        private void CustomizeViewItem_AppModel(ViewItem viewItem) {
            var item = (StaticTextViewItem)viewItem;
            item.Text = "Saved in the Application Model";
        }

        // This code bypasses the Application Model.
        private void CustomizeViewItem_Direct(ViewItem viewItem) {
            var item = (StaticTextViewItem)viewItem;
            var labelControl =(LabelControl) item.Control;
            labelControl.Text = "Reset on Next Startup";
        }
    }
}

```

## Escribir un controlador de eventos: guardar o descartar cambios en el modelo de aplicación

Controle el evento  [View.ModelSaveving.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.View.ModelSaving)  Puede utilizar su parámetro para especificar si es necesario guardar o descartar los cambios.

Casos de uso para este evento:

-   Control centralizado sobre los cambios en el modelo de aplicación
-   Controlar los cambios realizados por los usuarios finales



```csharp
using DevExpress.ExpressApp;
using dxT941118.Module.BusinessObjects;

namespace dxT941118.Module.Win.Controllers {
    public class CustomWinController : ObjectViewController<DetailView, Contact> {
        protected override void OnActivated() {
            base.OnActivated();
            View.ModelSaving += View_ModelSaving;
        }

        private void View_ModelSaving(object sender, System.ComponentModel.CancelEventArgs e) {
            e.Cancel = true;
        }
    }
}
```

# Almacenamientos de modelos de aplicación


# Almacenar diferencias del modelo de aplicación en la base de datos


Al crear una nueva aplicación con el sistema de seguridad habilitado en el  [Asistente para soluciones](https://docs.devexpress.com/eXpressAppFramework/113624/installation-upgrade-version-history/visual-studio-integration/solution-wizard), la configuración del usuario final (diferencias de modelo) se  [almacena en la base de datos](https://docs.devexpress.com/eXpressAppFramework/403527/ui-construction/application-model-ui-settings-storage/change-application-model)  mediante el almacenamiento  **ModelDifferenceDbStore**, de forma predeterminada. En este tema se describe cómo habilitar esta característica en una aplicación existente, junto con cómo almacenar las diferencias de modelo compartido (configuración del administrador) en la base de datos.

>NOTA
El **sistema.Seguridad.  Principal.  Identidad de Windows.  Actualícese().  El valor Name** se utiliza como identificador de usuario (pasado a [IModelDifference.ID de usuario](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.IModelDifference.UserId)) cuando el sistema de seguridad está deshabilitado. Por lo tanto, puede habilitar **ModelDifferenceDb Store** para aplicaciones de formularios Windows con el sistema deseguridad deshabilitado utilizando el enfoque descrito aquí. Sin embargo, no se recomienda habilitar el  **almacén de base de datos Model Differencepara aplicaciones ASP.NET Web Forms no seguras, ya que el identificador de usuario será el**  mismo  para todos los usuarios de esta instancia. Las diferencias de modelo compartido se admiten para WinForms, ASP.NET Web Forms y ASP.NET Core Blazor cuando el sistema de seguridad está deshabilitado.

Edite el archivo WinModule.cs (_WinModule.vb_) ubicado en el módulo  _WinForms_  o en el proyecto de aplicación. En el método  [ModuleBase.Setup](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ModuleBase.Setup.overloads)  reemplazado, suscríbase a los eventos XafApplication.CreateCustomModelDifferenceStore y  [XafApplication.CreateCustomUserModelDifferenceStore.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.XafApplication.CreateCustomModelDifferenceStore)  [](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.XafApplication.CreateCustomUserModelDifferenceStore)En estos controladores de eventos, pase la instancia de  **ModelDifferenceDbStore**  al parámetro  **e.Store**. Pase el tipo ModelDifference al constructor  **ModelDifferenceDbStore**.  **Aquí, ModelDifference**  es un objeto persistente integrado del espacio de nombres DevExpress.Persistent.BaseImpl (para XPO) o del espacio de nombres  [DevExpress.Persistent.BaseImpl.EF](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.BaseImpl.EF)  (para Entity Framework) que implementa la interfaz  [IModelDifference](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.IModelDifference)[.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.BaseImpl)  Tenga en cuenta el último parámetro  _contextId_  del constructor (utilizado para inicializar la propiedad  [IModelDifference.ContextId](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.IModelDifference.ContextId)). En el módulo WinForms, configúrelo en "Win" para distinguir las diferencias de modelo creadas para el mismo usuario en diferentes plataformas.



```csharp
public sealed partial class MySolutionWindowsFormsModule : ModuleBase {
    private void Application_CreateCustomModelDifferenceStore(Object sender, CreateCustomModelDifferenceStoreEventArgs e) {
        e.Store = new ModelDifferenceDbStore((XafApplication)sender, typeof(ModelDifference), true, "Win");
        e.Handled = true;
    }
    private void Application_CreateCustomUserModelDifferenceStore(Object sender, CreateCustomModelDifferenceStoreEventArgs e) {
        e.Store = new ModelDifferenceDbStore((XafApplication)sender, typeof(ModelDifference), false, "Win");
        e.Handled = true;
    }
    //...
    public override void Setup(XafApplication application) {
        base.Setup(application);
        application.CreateCustomModelDifferenceStore += Application_CreateCustomModelDifferenceStore;
        application.CreateCustomUserModelDifferenceStore += Application_CreateCustomUserModelDifferenceStore;
    }
}

```

Edite el archivo WebModule.cs (_WebModule.vb_) ubicado en el proyecto de módulo ASP.NET formularios Web Forms, pero establezca el parámetro  _contextId_  en "_Web_" en lugar de "Win".



```csharp
public sealed partial class MySolutionAspNetModule : ModuleBase {
    private void Application_CreateCustomModelDifferenceStore(Object sender, CreateCustomModelDifferenceStoreEventArgs e) {
        e.Store = new ModelDifferenceDbStore((XafApplication)sender, typeof(ModelDifference), true, "Web");
        e.Handled = true;
    }
    private void Application_CreateCustomUserModelDifferenceStore(Object sender, CreateCustomModelDifferenceStoreEventArgs e) {
        e.Store = new ModelDifferenceDbStore((XafApplication)sender, typeof(ModelDifference), false, "Web");
        e.Handled = true;
    }
    // ...
    public override void Setup(XafApplication application) {
        base.Setup(application);
        application.CreateCustomModelDifferenceStore += Application_CreateCustomModelDifferenceStore;
        application.CreateCustomUserModelDifferenceStore += Application_CreateCustomUserModelDifferenceStore;
    }
}

```

Edite el archivo  _BlazorModule.cs_  ubicado en el módulo ASP.NET Core Blazor o en el proyecto de aplicación, pero establezca el parámetro  _contextId_  en "Blazor" en lugar de "Win".


```csharp
public sealed partial class MySolutionBlazorModule : ModuleBase {
    private void Application_CreateCustomModelDifferenceStore(Object sender, CreateCustomModelDifferenceStoreEventArgs e) {
        e.Store = new ModelDifferenceDbStore((XafApplication)sender, typeof(ModelDifference), true, "Blazor");
        e.Handled = true;
    }
    private void Application_CreateCustomUserModelDifferenceStore(Object sender, CreateCustomModelDifferenceStoreEventArgs e) {
        e.Store = new ModelDifferenceDbStore((XafApplication)sender, typeof(ModelDifference), false, "Blazor");
        e.Handled = true;
    }

    // ...
    public override void Setup(XafApplication application) {
        base.Setup(application);
        application.CreateCustomModelDifferenceStore += Application_CreateCustomModelDifferenceStore;
        application.CreateCustomUserModelDifferenceStore += Application_CreateCustomUserModelDifferenceStore;
    }
}

```

>NOTA
Cuando se controla el evento  **Crearalmacén de diferencias de modelo personalizado, las diferencias de modelocompartido (configuración de administrador) se almacenan en la base de** datos. Cambios con el _modelo.El archivo XAFML_ ubicado en el proyecto de aplicación se omite si el registro de base de datos ya existe para las diferencias de modelo compartido. Para volver a cargar la configuración desde _el modelo.xafml_, habilite la interfaz de usuario administrativa y use la acción  **Importar diferencia**  de modelo compartido  (o elimine el registro "Diferencia de modelo compartido" y reinicie). Si este comportamiento es inapropiado, no controle este evento. Manéjelo solo en la configuración del proyecto RELEASE.

Abra el archivo Module.cs y agregue el código siguiente al constructor  _Module_:



```csharp
using DevExpress.Persistent.BaseImpl;
// ...
namespace SimpleProjectManager.Module {
    public sealed partial class SimpleProjectManagerModule : ModuleBase {
        public SimpleProjectManagerModule() {
            // ...
            AdditionalExportedTypes.Add(typeof(ModelDifference));
            AdditionalExportedTypes.Add(typeof(ModelDifferenceAspect));
        }
    }
}

```

Si usa Entity Framework Core, registre adicionalmente los tipos de entidad ModelDifference y  [ModelDifferenceAspect](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.BaseImpl.EF.ModelDifference)  dentro de  **su DbContext**.[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.BaseImpl.EF.ModelDifferenceAspect)


```csharp
using DevExpress.Persistent.BaseImpl.EF;
// ...
public class MyDbContext : DbContext {
    // ...
    public DbSet<ModelDifference> ModelDifferences { get; set; }
    public DbSet<ModelDifferenceAspect> ModelDifferenceAspects { get; set; }
}

```

Asegúrese de que todos los usuarios tengan acceso de lectura y escritura a los tipos  **ModelDifference**  y  **ModelDifferenceAspect.**



```csharp
public class Updater : ModuleUpdater {
    public override void UpdateDatabaseAfterUpdateSchema() {
        base.UpdateDatabaseAfterUpdateSchema();

        PermissionPolicyRole defaultRole = ObjectSpace.FirstOrDefault<PermissionPolicyRole>(role => role.Name == "Default");
        if(defaultRole == null) {
            defaultRole = ObjectSpace.CreateObject<PermissionPolicyRole>();
            defaultRole.Name = "Default";
            defaultRole.AddObjectPermissionFromLambda<PermissionPolicyUser>(SecurityOperations.Read, u => u.ID == (Guid)CurrentUserIdOperator.CurrentUserId(), SecurityPermissionState.Allow);
            defaultRole.AddNavigationPermission(@"Application/NavigationItems/Items/Default/Items/MyDetails", SecurityPermissionState.Allow);
            defaultRole.AddMemberPermissionFromLambda<PermissionPolicyUser>(SecurityOperations.Write, "ChangePasswordOnFirstLogon", u => u.ID == (Guid)CurrentUserIdOperator.CurrentUserId(), SecurityPermissionState.Allow);
            defaultRole.AddMemberPermissionFromLambda<PermissionPolicyUser>(SecurityOperations.Write, "StoredPassword", u => u.ID == (Guid)CurrentUserIdOperator.CurrentUserId(), SecurityPermissionState.Allow);
            defaultRole.AddTypePermissionsRecursively<PermissionPolicyRole>(SecurityOperations.Read, SecurityPermissionState.Deny);
            defaultRole.AddTypePermissionsRecursively<ModelDifference>(SecurityOperations.ReadWriteAccess, SecurityPermissionState.Allow);
            defaultRole.AddTypePermissionsRecursively<ModelDifferenceAspect>(SecurityOperations.ReadWriteAccess, SecurityPermissionState.Allow);
            // The 'Create' permission is additionally required if you use the Middle Tier Application Server
            defaultRole.AddTypePermissionsRecursively<ModelDifference>(SecurityOperations.Create, SecurityPermissionState.Allow);
            defaultRole.AddTypePermissionsRecursively<ModelDifferenceAspect>(SecurityOperations.Create, SecurityPermissionState.Allow);                
        }
        sampleUser.Roles.Add(defaultRole);
        // ...
        ObjectSpace.CommitChanges();
    }
    // ...
}

```

>PROPINA
Consulte el tema Cómo: Habilitar la interfaz de usuario administrativa para administrar las diferencias de modelo de los usuarios para  obtener información sobre cómo habilitar los elementos de la interfaz de usuario para administrar las diferencias de modelo almacenadas en la base de datos.[](https://docs.devexpress.com/eXpressAppFramework/113704/ui-construction/application-model-ui-settings-storage/application-model-storages/enable-the-administrative-ui-for-managing-users-model-differences)

>NOTA
La siguiente combinación de características no se admite cuando se usan juntas.
>
>-   Proveedor de espacio de  [objetosseguro o proveedor de espacio XPObject creado con el constructor con el parámetro Thread Safe establecido en true (este parámetro habilita la capa de datos](https://docs.devexpress.com/eXpressAppFramework/113437/data-security-and-safety/security-system/security-tiers/2-tier-security-integrated-mode-and-ui-level/change-the-client-side-security-mode-from-ui-level-to-integrated-in-xpo-applications)  [segura desubprocesos](https://docs.devexpress.com/XPO/DevExpress.Xpo.ThreadSafeDataLayer)).[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Xpo.XPObjectSpaceProvider)
>-   Las diferencias de modelo en toda la aplicación se  **almacenan en la base de datos** mediante la [[XafApplication.CreateCustomModelDifferenceStore](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.XafApplication.CreateCustomModelDifferenceStore) (aún puede almacenar diferencias específicas del usuario en la base de datos mediante la [XafApplication.CreateCustomUserModelDifferenceStore](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.XafApplication.CreateCustomUserModelDifferenceStore).
>-   [Custom Persistent Fields](https://docs.devexpress.com/eXpressAppFramework/113583/business-model-design-orm/types-info-subsystem/use-metadata-to-customize-business-classes-dynamically) declarados en las diferencias  _del modelo de nivel de aplicación_.
>
>En esta configuración, la aplicación carga información sobre campos persistentes personalizados de la base de datos y, a continuación, actualiza el esquema de la base de datos. Sin embargo, una capa de datos segura para subprocesos no admite la modificación del modelo de datos después de establecer la conexión de base de datos.


# Cómo: Convertir diferencias de modelo de aplicación entre versiones debido a cambios estructurales en el nodo


En este tema se describe qué hacer si publica una actualización para la aplicación y el modelo de aplicación es incompatible con la versión anterior.

## Convertidor XML o actualizador de nodos: cómo elegir

### Escenario básico

Tiene previsto lanzar una nueva versión de la aplicación. La actualización contiene cambios en el modelo de aplicación. Por ejemplo, ha cambiado el nombre de una propiedad de nodo o ha movido un nodo a otra posición dentro del árbol. El objetivo es asegurarse de que la aplicación permanezca operativa y que los usuarios no pierdan los cambios realizados en el modelo de aplicación mientras usaban la versión anterior.

### Posibles problemas de actualización

Al iniciarse, una aplicación XAF intenta cargar el modelo de aplicación. Busca los cambios realizados por los usuarios en el modelo e intenta aplicarlos. Si la estructura del modelo de aplicación ha cambiado desde la última versión, XAF no puede procesar las diferencias y muestra un mensaje de error.

### Solución

Siempre que planee cambiar la estructura del modelo de aplicación, asegúrese de implementar una forma de convertir las diferencias. Puede utilizar las dos técnicas siguientes.

1.  **Convertidor XML**. Implemente la interfaz  [IModelXmlConverter](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Updating.IModelXmlConverter)  para procesar archivos XAFML (un formato XML personalizado). Este método procesa los nodos uno por uno. Es posible que no se adapte a sus necesidades si necesita administrar varios nodos a la vez.
2.  **Actualizador de nodos**. Implemente la interfaz  [IModelNodeUpdater<T>](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.Core.IModelNodeUpdater-1). Dichos actualizadores pueden realizar conversiones mucho más versátiles.

Revise las siguientes secciones para ver ejemplos.

## Ejemplos de convertidores XML

En esta sección se enumeran los escenarios comunes de actualización del modelo de aplicación que puede resolver con un convertidor XML.

En todos los ejemplos, debe ampliar el módulo con la interfaz  [IModelXmlConverter](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Updating.IModelXmlConverter)  e implementar el método  [ConvertXml().](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Updating.IModelXmlConverter.ConvertXml(DevExpress.ExpressApp.Updating.ConvertXmlParameters))  XAF llama a este método para cada nodo del modelo de aplicación para que pueda ajustar la configuración según sea necesario.

### Ejemplo: Cambio de tipo de propiedad

En este ejemplo se muestra cómo procesar un cambio en un tipo de propiedad.

Supongamos que la versión anterior de un módulo extendía Application Model con una propiedad  **Mode**. Los usuarios pueden usar las cadenas "Simple" o "Avanzado" como valores de propiedad.



```csharp
using DevExpress.ExpressApp.Model;

public interface IModelMyOptions{
    // accepts strings "Simple" or "Advanced"
    string Mode { get; set; }
}
public sealed partial class MyModule : ModuleBase {
    //...
    public override void ExtendModelInterfaces(ModelInterfaceExtenders extenders) {
        base.ExtendModelInterfaces(extenders);
        extenders.Add<IModelOptions, IModelMyOptions>();
    }
}

```

La nueva versión cambia el tipo de propiedad a una enumeración. (Tenga en cuenta que el código también cambia el nombre de uno de los modos de "Simple" a "Básico").



```csharp
public enum OperatingMode { Basic, Advanced }

public interface IModelMyOptions {
    // now accepts enumeration values
    OperatingMode Mode { get; set; }
}

```

Puede implementar el siguiente convertidor XML para garantizar la transición correcta a la nueva versión.



```csharp
using DevExpress.ExpressApp.Model;
using DevExpress.ExpressApp.Updating;

public sealed partial class MyModule : ModuleBase, IModelXmlConverter {
    //...
    public void ConvertXml(ConvertXmlParameters parameters) {
        // Check if this is the Options node.
        if(typeof(IModelOptions).IsAssignableFrom(parameters.NodeType)) {
            // Check if the user assigned a value to Mode.
            string property = "Mode";
            if(parameters.ContainsKey(property)) {
                // Retrieve the value.
                string value = parameters.Values[property];
                // If it's a legacy value, convert.
                if(!Enum.IsDefined(typeof(OperatingMode), value)) {
                    switch(value.ToLower()) {
                        case "advanced":
                            parameters.Values[property] = OperatingMode.Advanced.ToString();
                            break;
                        default:
                            parameters.Values[property] = OperatingMode.Basic.ToString();
                            break;                            
                    }                                  
                }                    
            }
        }
    }
}

```

### Ejemplo: Cambio de nombre de propiedad

Supongamos que la versión anterior del módulo extendía el modelo de aplicación con la propiedad  **Mode**. La nueva versión cambia el nombre de esta propiedad a  **OperatingMode**.

Puede utilizar el siguiente convertidor XML para controlar esta transición:



```csharp
using DevExpress.ExpressApp.Model;
using DevExpress.ExpressApp.Updating;

public sealed partial class MyModule : ModuleBase, IModelXmlConverter {
    //...
    public void ConvertXml(ConvertXmlParameters parameters) {
        if(typeof(IModelOptions).IsAssignableFrom(parameters.NodeType)) {
            string oldProperty = "Mode";
            string newProperty = "OperatingMode";
            if(parameters.ContainsKey(oldProperty)) {
                parameters.Values[newProperty] = parameters.Values[oldProperty];
                parameters.Values.Remove(oldProperty);
            }
        }
    }
}

```

## Ejemplos de Node Updater

En esta sección se enumeran los escenarios comunes de actualización del modelo de aplicación que puede resolver con un actualizador de nodos.

Para crear un actualizador de nodos, siga estos pasos:

-   Extienda cualquier clase con la interfaz  [IModelNodeUpdater<T>](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.Core.IModelNodeUpdater-1). El parámetro de tipo genérico indica el tipo de nodo del modelo de aplicación.
-   Implemente el método  [UpdateNode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.Core.IModelNodeUpdater-1.UpdateNode(-0-DevExpress.ExpressApp.Model.IModelApplication))  de la interfaz. Los parámetros de este método le permiten acceder al nodo y a todo el modelo de aplicación. Esto significa que puede acceder a cualquier nodo de su código y, por lo tanto, implementar conversiones de cualquier complejidad.
-   Reemplace el método  [ModuleBase.AddModelNodeUpdaters para registrar la clase Node Updater.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ModuleBase.AddModelNodeUpdaters(DevExpress.ExpressApp.Model.Core.IModelNodeUpdaterRegistrator))

### Ejemplo: Cambio de ubicación de la propiedad

Supongamos que la versión anterior de un módulo extendía el nodo  **Opciones**  del modelo de aplicación con una propiedad  **de cadena OperatingMode**.



```csharp
public interface IModelMyOptions {
    OperatingMode OperatingMode { get; set; }
}
public enum OperatingMode { Basic, Advanced }

```

La nueva versión mueve la propiedad a un nodo secundario recién introducido:



```csharp
using DevExpress.ExpressApp.Model;

public interface IModelMyOptions {
    IModelChildOptions ChildOptions { get; }
}
public interface IModelChildOptions : IModelNode {
    OperatingMode OperatingMode { get; set; }
}
public enum OperatingMode { Basic, Advanced }

```

Cree un actualizador de nodos para garantizar la transición correcta a la nueva versión. Puede implementar la interfaz en su clase de módulo como se muestra a continuación.



```csharp
using DevExpress.ExpressApp.Model;
using DevExpress.ExpressApp.Model.Core;

// Node Updater interface will process the Options node.
public sealed partial class MyModule : ModuleBase, IModelNodeUpdater<IModelOptions> {
    //...
    public void UpdateNode(IModelOptions node, IModelApplication application) {
        string myProperty = "OperatingMode";
        // Try and find the property at its previous location.
        if(node.HasValue(myProperty)) {
            // Retrieve the value.
            string value = node.GetValue<string>(myProperty);
            // Clear the legacy property.
            node.ClearValue(myProperty);
            // Assign the value to the new property.
            ((IModelMyOptions)node).ChildOptions.OperatingMode = 
                (OperatingMode)Enum.Parse(typeof(OperatingMode), value);
        }
    }        
}

```

Reemplace el método  [ModuleBase.AddModelNodeUpdaters](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ModuleBase.AddModelNodeUpdaters(DevExpress.ExpressApp.Model.Core.IModelNodeUpdaterRegistrator))  y registre el actualizador de nodos.



```csharp
using DevExpress.ExpressApp.Model;
using DevExpress.ExpressApp.Model.Core;

public sealed partial class MyModule : ModuleBase, IModelNodeUpdater<IModelOptions> {
    //...
    public override void AddModelNodeUpdaters(IModelNodeUpdaterRegistrator updaterRegistrator) {
        base.AddModelNodeUpdaters(updaterRegistrator);
        updaterRegistrator.AddUpdater<IModelOptions>(this);
    }    
}
```


# Enable the Administrative UI to manage End-User Settings in the Database


This topic describes how to enable UI elements you can use to manage Model Differences  [stored in the database](https://docs.devexpress.com/eXpressAppFramework/113698/ui-construction/application-model-ui-settings-storage/application-model-storages/store-the-application-model-differences-in-the-database).

The Administrative UI allows application administrators to manage user settings in user accounts - create, copy, export and reset Model Differences. Follow the steps below to enable this UI in your WinForms, ASP.NET WebForms, or ASP.NET Core Blazor application:

1.  Open the  _Module.cs_  file and add the following code to the Module constructor:
    

    
    ```csharp
    using DevExpress.Persistent.BaseImpl;
    // ...
    namespace MainDemo.Module {
      public sealed partial class MainDemoModule : ModuleBase {
         public MainDemoModule() {
            // ...
            AdditionalExportedTypes.Add(typeof(ModelDifference));
            AdditionalExportedTypes.Add(typeof(ModelDifferenceAspect));
         }
         // ...
      }
    }
    
    ```
    
    If you use the Entity Framework Core, ensure that your  **DbContext**  contains the following lines:
    

    
    ```csharp
    using DevExpress.Persistent.BaseImpl.EF;
    // ...
    public class MyDbContext : DbContext {
       // ...
       public DbSet<ModelDifference> ModelDifferences { get; set; }
       public DbSet<ModelDifferenceAspect> ModelDifferenceAspects { get; set; }
    }
    
    ```
    
2.  Run the  [Model Editor](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor)  and  [create a new Navigation Item](https://docs.devexpress.com/eXpressAppFramework/402131/getting-started/in-depth-tutorial-blazor/customize-navigation-between-views/add-an-item-to-navigation-control)  for the  **ModelDifference_ListView**  List View.    
![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/75b8fd99-a3b8-49cd-bcca-79dac2579a32)
    

Run the application and click this Navigation Item to open the Model Difference List View. Ensure that the Model Differences management Actions are available in the  **Tools**  category.

**WinForms**

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/a6745f38-b9d8-4454-be56-5c174116ab4f)

**ASP.NET WebForms**

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/54177445-7840-4dd5-882d-6914d347c02c)

**ASP.NET Core Blazor**

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/9d74083c-0aec-48d6-9cb3-cd77db552c1f)

Users who used the application at least once have initialized Model Differences. The List View lists these Model Differences. This View also contains the  **Shared Model Difference**  record that stores global settings. Click  **Create Model Differences**  to create Model Differences for unlisted users. Click  **Import Shared Model Difference**  to load shared Model Differences created in Visual Studio (the  _Model.xafml_  file). You can also copy, export, and reset a selected record.

>NOTE
>
>-   The **Export Model Differences** Action saves model differences to the application’s _ExportedModelDifferences_ subfolder. Use the **ModelDifferenceViewController.ExportedModelDifferencesPath** property to change the folder.
>-   Restart an ASP.NET Web Forms application on the web server to apply changes if you changed its global settings (for example, using the **Import Shared Model Difference** Action).


# Combinar la configuración de usuario final de una aplicación de FormulariosWindows del entorno de desarrollo al de producción


Los usuarios finales pueden personalizar una interfaz de usuario (UI) de aplicación XAF en tiempo de ejecución con facilidad. El Administrador de diseño, el Selector de columnas y otras capacidades permiten a los usuarios finales configurar una interfaz de usuario de una manera de "lo que ves es lo que obtienes". Pero cuando usted, como desarrollador, personaliza una interfaz de usuario en el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor), tiene que lidiar con índices, anchos, alturas, grupos, etc. Por lo tanto, es posible que desee personalizar una interfaz de usuario como usuario final y, a continuación, combinar los cambios en una de las capas del  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)  de la solución XAF. En este tema se describe cómo realizar esta tarea mediante la  [herramienta Combinación de modelos](https://docs.devexpress.com/eXpressAppFramework/113334/installation-upgrade-version-history/visual-studio-integration/model-merge-tool). Como ejemplo, la configuración del orden de las columnas se fusionará a partir de las diferencias de usuario en la capa de  [proyecto del módulo](https://docs.devexpress.com/eXpressAppFramework/118045/application-shell-and-base-infrastructure/application-solution-components/application-solution-structure). Sin embargo, puede usar el mismo enfoque para combinar cualquier personalización del usuario final. Por ejemplo:

-   Vista de lista de columnas visibilidad, anchura, agrupación, configuración de filtro;
-   [Diseños de](https://docs.devexpress.com/eXpressAppFramework/112817/ui-construction/views/layout/view-items-layout-customization)  panel y vista detallada;
-   [Configuración de gráficos](https://docs.devexpress.com/eXpressAppFramework/113302/analytics/chart-module)  y  [dinámicas](https://docs.devexpress.com/eXpressAppFramework/113303/analytics/pivot-grid-module).


>NOTA
La herramienta de combinación de modelos solo admite aplicaciones de formularios Windowscon diferencias de modelo de usuario final almacenadas en un archivo XAFML.

## Personalizar una interfaz de usuario en tiempo de ejecución

En este tema usaremos la aplicación MainDemo, ubicada en la carpeta %_PUBLIC%\Documents\DevExpress Demos  23.1\Components\XAF\MainDemo._  Abra la solución MainDemo (C# o VB), establezca  **MainDemo.Win**  como proyecto de inicio y ejecute la aplicación de Windows Forms. Seleccione el elemento  **Reanudar**  navegación. Verá la vista de lista del objeto  **Reanudación**. Utilice arrastrar y soltar para intercambiar la columna  **Archivo**  con  **Contacto**.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/797e3216-90b4-4cf5-9640-d536b27a6741)

Cierre la aplicación de Windows Forms. Las personalizaciones realizadas se guardan en el archivo  _Model.User.xafml_  ubicado en la carpeta de resultados del proyecto (_bin\Debug\_  de forma predeterminada).



```xml
<ListView Id="Resume_ListView">
  <Columns>
    <ColumnInfo Id="Contact" Index="0" />
    <ColumnInfo Id="File" Index="1" />
  </Columns>
</ListView>

```

## Combinar diferencias de usuario en la capa de proyecto de módulo

Ahora vamos a combinar las personalizaciones del usuario final en el modelo de aplicación del proyecto de módulo. En el  **Explorador de soluciones**, haga clic con el botón secundario en el proyecto de aplicación  **MainDemo.Win**  y haga clic en  **Combinar modelo de usuario**. En el cuadro de diálogo  **Abrir**  invocado, elija el archivo  _Model.User.xafml._

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/aa8aaef2-58ed-4ea6-8d1e-c9870f3bfc04)

Como resultado, se invoca el cuadro de diálogo  **Herramienta de combinación de modelos**. Desplácese hasta el nodo  **Resume_ListView**  de la lista de árbol. Este título de nodo se muestra en negrita, ya que el nodo contiene personalizaciones. Utilice la casilla de verificación situada a la izquierda para seleccionar este nodo. En el menú desplegable siguiente, seleccione  **MainDemo.Module**  y haga clic en  **Combinar**. Las diferencias del nodo  **Resume_ListView**  se combinarán en el proyecto  **MainDemo.Module.**

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/e3080570-7a51-4176-afd9-d58e60c70b40)

Por último, haga clic en  **Guardar**  para conservar los cambios y cierre el cuadro de diálogo  **Herramienta de combinación de modelos**.

>NOTA
>
>-   Puede seleccionar varios nodos a la vez y repetir la operación de combinación tantas veces como sea necesario, antes de hacer clic en **Guardar**. Si una parte de las diferencias seleccionadas no se puede aplicar al objetivo seleccionado, se mostrará un mensaje de advertencia. En este caso, puede probar con otro objetivo (por  ejemplo, **MainDemo.Module.Win**).
>-   Si la aplicación  ya está implementada y uno de los usuarios finales tiene un diseño cuidadosamente diseñado, es posible que desee usar este diseño en una nueva versión de la aplicación. En este caso, simplemente copie el _modelo.Usuario.  xafml_  de la  estación de trabajo del usuario final y seleccione este archivo en el cuadro de diálogo  **Abrir** al ejecutar la **Model Merge Tool**..

## Comprobar cambios

Después de la combinación, puede ver las personalizaciones de nodo  **Resume_ListView**  en el Editor de modelos invocadas para el proyecto  **MainDemo.Module.**

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/83ea6717-ac02-4967-9729-370c286d6e17)

Para probar los cambios en tiempo de ejecución, restablezca las diferencias del modelo de usuario. La forma más sencilla de hacerlo es eliminar el archivo  _Model.User.xafml_  de la carpeta de resultados del proyecto de aplicación. A continuación, ejecute la aplicación de Windows Forms para ver que un diseño se personaliza según sea necesario.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/4198110b-18da-4cdc-89f0-e2c0ccd0208b)

A medida que las diferencias se trasladaron a un módulo independiente de la plataforma, puede ver que las personalizaciones también afectan a la aplicación ASP.NET formularios Web Forms.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/dc293c10-4b7f-4b47-9cb5-a201a53edd4d)


# Información general sobre los elementos de la interfaz de usuario

En este artículo se describen los elementos de la interfaz de usuario de XAF. Los diagramas siguientes muestran elementos abstractos y controles reales, representan su ubicación en la interfaz de usuario e ilustran su estructura.


## Ubicación de los elementos

En XAF, un control de interfaz de usuario y una entidad abstracta relacionada definen un elemento de interfaz de usuario. Las siguientes imágenes muestran las ubicaciones de los elementos de interfaz de usuario en aplicaciones de formularios WinForms y ASP.NET formularios Web Forms:

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/bf01e7ad-f759-4fe1-89af-fc8c1f2285aa)

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/c67bb41e-0c70-4af1-aa45-60a645aee4c8)

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/cdc948ef-256c-49d6-b57d-7d42de332e81)

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/d873d676-9b20-48c0-9496-907ae92b49f9)

## Estructura de los elementos

Los siguientes gráficos muestran la estructura de estos elementos:

**Vista detallada**

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/5f3cfbbb-c5c1-46b9-a4f1-6d26dac43ec2)

**Vista de lista y vista de lista anidada**

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/076b3a5f-1a82-4d66-969c-eb8b9bfe0f54)


# Cómo: Incluir un control de interfaz de usuario personalizado que no está integrado de forma predeterminada (formularios Win, formulariosWebForms ASP.NET y ASP.NET  Core Blazor)


XAF usa controles estándar y DevExpress para todos los elementos de la interfaz de usuario, como editores de listas, editores de propiedades para tipos de datos predeterminados,  [elementos de vista](https://docs.devexpress.com/eXpressAppFramework/112612/ui-construction/view-items-and-property-editors), acciones,  [contenedores](https://docs.devexpress.com/eXpressAppFramework/112610/ui-construction/action-containers)  [de](https://docs.devexpress.com/eXpressAppFramework/113014/business-model-design-orm/data-types-supported-by-built-in-editors)  [acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions),  [plantillas](https://docs.devexpress.com/eXpressAppFramework/113189/ui-construction/list-editors), etc. Para escenarios no cubiertos por los elementos integrados de la interfaz de usuario XAF, XAF permite a los desarrolladores integrar cualquier otro control de interfaz de usuario de DevExpress, estándar (Microsoft) y de terceros en Views. En este tema se agrupan las soluciones por tareas populares de interfaz de usuario.

Estas tareas a menudo se realizan en un nivel de marco inferior y requieren más conocimiento y experiencia con XAF. Este es el mismo conocimiento requerido si tuviera que codificar una aplicación sin nuestro marco. Los beneficios relacionados con XAF y el ahorro de tiempo son mínimos para aquellos que desean integrar controles personalizados. Como regla general, primero debe saber cómo resolver y, opcionalmente, implementar su tarea en una aplicación que no sea XAF. Esto hace que la integración de XAF sea mucho más fácil. Para obtener más información, consulte el artículo siguiente:  [Advanced UI Customization Tips](https://www.devexpress.com/products/net/application_framework/xaf-considerations-for-newcomers.xml#advanced-ui-customization) .

Para incluir un control de interfaz de usuario personalizado que no esté integrado de forma predeterminada, implemente un  [ListEditor](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Editors.ListEditor),  [PropertyEditor](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Editors.PropertyEditor)  o  [ViewItem](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Editors.ViewItem)  que ajuste el control y sirva como adaptador para la infraestructura XAF. También puede  [personalizar una plantilla](https://docs.devexpress.com/eXpressAppFramework/112696/ui-construction/templates/template-customization)  para colocar el control en una ubicación específica. Consulte la tabla a continuación para obtener detalles sobre las diferentes soluciones.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/ac307523-6fa8-4b3c-b40f-4131c6640c0d)
![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/c130e0ac-320a-4f0a-82e2-8f2a95b16611)
![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/106fc7db-9341-476f-bdbf-0e032b79eaf2)
![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/199a08b1-3453-4734-93c3-b2bee39cb638)
![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/0894a90c-837d-4c30-985e-df670f8476ab)
![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/a05bd839-fb12-460b-8f35-3abf62c3616c)


>NOTA
>
>Controles de formularios Web Forms de ASP.NET que utilizan el Administrador de secuencias de comandos de cliente. [El método RegisterStartupScript  ](https://docs.microsoft.com/en-us/dotnet/api/system.web.ui.clientscriptmanager.registerstartupscript)  no se puede integrar con los ejemplos anteriores. Si experimenta alguna dificultad al integrar controles de formularios Web Forms ASP.NET, póngase en contacto con nuestro equipo de soporte técnico.

# Controladores y acciones (comandos de menú)

**eXpressApp Framework**  genera automáticamente la interfaz de usuario en función de su modelo de negocio. Esta interfaz de usuario contiene características integradas para trabajar con datos: filtrado, informes, navegación, etc. Estas características pueden ser suficientes para una aplicación empresarial simple. Sin embargo, las aplicaciones complejas pueden exigir un conjunto de funcionalidades más extenso. Para implementar características adicionales,  **eXpressApp Framework**  proporciona el concepto de Controladores y Acciones. Este concepto permite la implementación tanto de las características internas de la aplicación como de la interacción con el usuario final. Los temas de esta sección le familiarizarán con estos conceptos y le permitirán usarlos en sus aplicaciones.

#Controladores y acciones integrados


# Controladores integrados y acciones en el módulo del sistema


En este tema se enumeran  [los controladores](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers)  integrados y sus  [acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  suministradas con los módulos base del sistema, formularios Windows Forms y ASP.NET formularios Web Forms. Tenga en cuenta que puede encontrar más información sobre controladores o acciones individuales en el Editor de  [modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor)  navegando a  **ActionDesign**  del  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)  |  **Controladores**  o  **ActionDesign**  |  **Nodo de acciones**, respectivamente.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/636296b0-5d11-4ff4-b3be-2e003bf620fe)

>NOTA
>En este tema, nos referimos a las acciones integradas mediante sus valores de propiedad ActionBase.Id. Utilice el Editor de modelos para averiguar qué título se asigna a una acción.


## Crear, leer, actualizar y eliminar (CRUD)

### CheckDeletedObjectController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Activado para vistas detalladas.

Hace que la vista detallada actual sea de sólo lectura y muestra el mensaje "Los datos se muestran en modo de solo lectura, porque se han eliminado" si se ha eliminado el objeto de la vista.

----------

### DeleteObjectsViewController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **Eliminar**.

Activado para todas las vistas. Contiene la acción  **Eliminar**. Esta acción permite a los usuarios finales eliminar el objeto actual que se muestra en una vista detallada o los objetos seleccionados actualmente en una vista de lista. Tenga en cuenta que los objetos eliminados de colecciones anidadas no se eliminan de inmediato. Se eliminan cuando un usuario final guarda todo el objeto raíz.

**Vea también:**  [DeleteObjectsViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.DeleteObjectsViewController)  |  [DeleteObjectsViewController.DeleteAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.DeleteObjectsViewController.DeleteAction)  |  [DeleteObjectsViewController.AutoCommit](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.DeleteObjectsViewController.AutoCommit)

----------

### DependentEditorController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Activado para vistas detalladas. Actualiza los campos que muestran los valores de las propiedades de referencia de un objeto cuando se cambia una propiedad de referencia.

----------

### LinkDialogController
Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Un descendiente de  [DialogController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#dialogcontroller). Activado en la ventana emergente de la acción de  **enlace**  si admite la  [funcionalidad de búsqueda](https://docs.devexpress.com/eXpressAppFramework/112925/ui-construction/controllers-and-actions/actions/how-to-add-a-search-action-to-lookup-property-editors-and-link-pop-up-windows). Agrega  [LinkNewObjectController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#linknewobjectcontroller)  a la ventana, invocado al presionar el botón  **Nuevo**. Ejecuta la acción  **FullTextSearch**  para recuperar el objeto recién creado en la colección de la vista de lista actual.

**Véase también:**  [LinkUnlinkController.LinkAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.LinkUnlinkController.LinkAction)  |  [FilterController.FullTextFilterAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.FilterController.FullTextFilterAction)  |  [Cómo: Agregar una acción de búsqueda a editores de propiedades de búsqueda y ventanas emergentes de vínculos](https://docs.devexpress.com/eXpressAppFramework/112925/ui-construction/controllers-and-actions/actions/how-to-add-a-search-action-to-lookup-property-editors-and-link-pop-up-windows)

----------

### LinkNewObjectController
Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Un descendiente de  [DialogController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#dialogcontroller). Se activa en la ventana emergente que se invoca cuando se pulsa el botón  **Nuevo**  en la ventana emergente de la acción de  **enlace**. Cuando se presiona el botón  **OK**, este controlador guarda el objeto recién creado y lo pasa al  [LinkDialogController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#linkdialogcontroller), para que se pueda seleccionar en la ventana emergente de la acción de  **vínculo**.

**Véase también:**  [LinkUnlinkController.LinkAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.LinkUnlinkController.LinkAction)  |  [Cómo: Agregar una acción de búsqueda a editores de propiedades de búsqueda y ventanas emergentes de vínculos](https://docs.devexpress.com/eXpressAppFramework/112925/ui-construction/controllers-and-actions/actions/how-to-add-a-search-action-to-lookup-property-editors-and-link-pop-up-windows)

----------

### LinkToListViewController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Para uso interno. Crea un objeto  **Link**  utilizado por  [DetailViewLinkController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#detailviewlinkcontroller)  para sincronizar los cambios realizados en una vista detallada invocada desde la vista de lista.

----------

### LinkUnlinkController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **Enlazar**,  **Desvincular**.

Activado para vistas de lista anidadas. La acción  **Vincular**  permite a los usuarios finales agregar un objeto existente a la colección anidada actual. Los objetos a elegir se muestran mediante una vista de lista en una ventana emergente. La acción  **Desvincular**  elimina las referencias al objeto seleccionado en la colección anidada actual. Los cambios realizados en una colección no se guardan inmediatamente; Se guardan cuando se guarda el objeto raíz. Este comportamiento se reemplaza en el  [WebLinkUnlinkController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#weblinkunlinkcontroller)  específico de formularios Web Forms ASP.NET.

**Ver también:**  [LinkUnlinkController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.LinkUnlinkController)  |  [LinkUnlinkController.LinkAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.LinkUnlinkController.LinkAction)  |  [LinkUnlinkController.UnlinkAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.LinkUnlinkController.UnlinkAction)  |  [LinkUnlinkController.AutoCommit](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.LinkUnlinkController.AutoCommit)

----------

### ModificationsController
Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **Cancelar**,  **Guardar**,  **GuardarAndCerrar**,  **GuardarAndNuevo**.

Activado para vistas detalladas. Crea y administra el estado activo y habilitado de las acciones  **Cancel**,  **Save****, SaveAndClose**  y  **SaveAndNew**.

**Ver también:**  [ModificationsController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ModificationsController)  |  [ModificationsController.CancelAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ModificationsController.CancelAction)  |  [ModificacionesControlador.GuardarAcción](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ModificationsController.SaveAction)  |  [ModificationsController.SaveAndCloseAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ModificationsController.SaveAndCloseAction)  |  [ModificacionesControlador.GuardarAndNuevoAcción](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ModificationsController.SaveAndNewAction)  |  [ModificacionesControlador.ModificacionesCheckingMode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ModificationsController.ModificationsCheckingMode)  |  [ModificacionesControlador.ModificacionesManejoModo](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ModificationsController.ModificationsHandlingMode)

----------

### NewObjectViewController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **Nuevo**.

Activado para todas las vistas. La  **nueva**  acción permite a los usuarios finales crear un nuevo objeto del tipo seleccionado de la lista de tipos predefinidos. Para especificar tipos predefinidos en el Editor de modelos, agregue nodos secundarios al nodo  **CreatableItems**. Para especificar un tipo predefinido en el código, use el atributo  **NavigationItem**  o controle los eventos  **CollectCreatableItemTypes**  y  **CollectDescendantTypes**  del controlador.

**Vea también:**  [NewObjectViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController)  |  [NewObjectViewController.NewObjectAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController.NewObjectAction)  |  [IModelCreatableItems](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelCreatableItems)  |  [NavigationItemAttribute](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.NavigationItemAttribute)  |  [NewObjectViewController.CollectDescendantTypes](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController.CollectDescendantTypes)  |  [Cómo: Personalizar la lista de elementos de la nueva acción](https://docs.devexpress.com/eXpressAppFramework/112915/ui-construction/controllers-and-actions/actions/how-to-customize-the-new-actions-items-list)

----------

### WebDeleteObjectsViewController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: heredadas.

Un descendiente  [de DeleteObjectsViewController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#deleteobjectsviewcontroller). Cambia el valor predeterminado de la propiedad  [DeleteObjectsViewController.AutoCommit](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.DeleteObjectsViewController.AutoCommit)  a  **True**, de modo que los objetos eliminados de cualquier colección se eliminen a la vez.

----------

### WebLinkUnlinkController
Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: heredadas.

El controlador se hereda de  [LinkUnlinkController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#linkunlinkcontroller). Establece la propiedad  [LinkUnlinkController.AutoCommit](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.LinkUnlinkController.AutoCommit)  en  **true**  cuando el  [ShowViewStrategy.CollectionsEditMode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.ShowViewStrategy.CollectionsEditMode)  actual es  [ViewEditMode.View.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Editors.ViewEditMode)

**Véase también:**  [ShowViewStrategy.CollectionsEditMode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.ShowViewStrategy.CollectionsEditMode)

----------

### WebModificationsController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones:  **SwitchToEditMode**  y heredadas.

El controlador se hereda de  [ModificationsController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#modificationscontroller). Activado para vistas de objetos. La acción  **SwitchToEditMode**, implementada en este controlador, permite a los usuarios finales cambiar al modo de edición cuando se muestra una vista detallada en modo de vista. Administra el estado activo de todas sus acciones, en función del valor de la propiedad  [DetailView.ViewEditMode de](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.DetailView.ViewEditMode)  la vista actual.

**Ver también:**  [WebModificationsController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.SystemModule.WebModificationsController)  |  [WebModificationsController.EditAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.SystemModule.WebModificationsController.EditAction)

----------

### WebNewObjectViewController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones:  **QuickCreateAction**  y heredadas.

El controlador se hereda de  [NewObjectViewController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#newobjectviewcontroller). Invalida el método  **UpdateActionsState**  para rellenar la colección  **Items**  de  [NewObjectViewController.NewObjectAction Action.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController.NewObjectAction)  Sólo el tipo de objeto de la vista actual y sus descendientes se agregan a la colección.

Contiene la acción  **QuickCreate**, cuya colección Items incluye sólo los elementos que representan los nodos secundarios del nodo  [IModelCreatableItems](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelCreatableItems)  del modelo de aplicación.  La colección se rellena en el método  **UpdateActionsState**.

**Vea también:**  [WebNewObjectViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.SystemModule.WebNewObjectViewController)  |  [WebNewObjectViewController.QuickCreateAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.SystemModule.WebNewObjectViewController.QuickCreateAction)

----------

### WinModificationsController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: heredadas.

El controlador se hereda de  [ModificationsController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#modificationscontroller). Activado para vistas de objetos.

**Vea también:**  [WinModificationsController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.SystemModule.WinModificationsController)

----------

### WinNewObjectViewController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: heredadas.

El controlador se hereda de  [NewObjectViewController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#newobjectviewcontroller). Invalida el método  **UpdateActionsState**  para rellenar la colección  [ChoiceActionBase.Items](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ChoiceActionBase.Items)  de la  **nueva**  acción. El tipo de objeto de la vista actual, sus descendientes y los tipos enumerados en el nodo  **CreatableItems**  del modelo de aplicación se agregan a la colección.

**Vea también:**  [WinNewObjectViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.SystemModule.WinNewObjectViewController)  |  [NewObjectViewController.NewObjectAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController.NewObjectAction)  |  [IModelCreatableItems](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelCreatableItems)

----------

## Búsqueda y filtrado

### FilterController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **SetFilter**,  **FullTextSearch**.

Activado para vistas de lista. La acción  **SetFilter**  permite a los usuarios finales seleccionar uno de los filtros predefinidos creados para la vista de lista actual. La acción  **FullTextSearch**  permite a los usuarios finales buscar objetos que coincidan con el texto introducido. Además de las acciones, este controlador filtra el origen de datos de la vista de lista actual según los criterios especificados en el modelo de aplicación.

**Ver también:**  [FilterController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.FilterController)  |  [FilterController.SetFilterAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.FilterController.SetFilterAction)  |  [FilterController.FullTextFilterAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.FilterController.FullTextFilterAction)  |  [Nodo modelo de aplicación de filtros](https://docs.devexpress.com/eXpressAppFramework/112992/filtering/in-list-view/filters-application-model-node)  |  [Acción FullTextSearch](https://docs.devexpress.com/eXpressAppFramework/112997/filtering/in-list-view/full-text-search-action)  |  [Propiedad Criteria en el modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112990/filtering/in-list-view/criteria-property-in-the-application-model)

----------

### FindLookupDialogController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Un descendiente de  [DialogController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#dialogcontroller). Se activa en la ventana de búsqueda del Editor de propiedades de búsqueda si la funcionalidad de búsqueda está habilitada.

Agrega  [FindLookupNewObjectDialogController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#findlookupnewobjectdialogcontroller)  a la ventana invocada al presionar el botón  **Nuevo**. Ejecuta la acción  **FullTextSearch**  para seleccionar el objeto recién creado en la colección de la vista de lista actual.

**Vea también:**  [IModelOptions.LookupSmallCollectionItemCount](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelOptions.LookupSmallCollectionItemCount)  |  [Cómo: Agregar una acción de búsqueda a editores de propiedades de búsqueda y ventanas emergentes de vínculos](https://docs.devexpress.com/eXpressAppFramework/112925/ui-construction/controllers-and-actions/actions/how-to-add-a-search-action-to-lookup-property-editors-and-link-pop-up-windows)

----------

### FindLookupNewObjectDialogController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Un descendiente de  [DialogController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#dialogcontroller). Se activa en la ventana emergente que se invoca al pulsar el botón  **Nuevo**  en la ventana de búsqueda del Editor de propiedades de búsqueda. Cuando se presiona el botón  **Aceptar**, este controlador guarda el objeto recién creado y lo pasa al  [FindLookupDialogController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#findlookupdialogcontroller), para que se pueda seleccionar en la ventana de búsqueda del Editor de propiedades de búsqueda.

----------

## Navegación

### RecordsNavigationController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **PreviousObject**,  **NextObject**.

Activado para todas las vistas. La acción  **PreviousObject**  está pensada para navegar al objeto anterior en el origen de la colección. Cuando se utiliza esta acción para una vista de lista, se selecciona el objeto anterior en el editor de la vista. Cuando se utiliza esta acción en una vista de detalle que muestra un objeto seleccionado actualmente en una vista de lista, el objeto anterior en el editor de la vista de lista se muestra en la vista de detalles. La acción  **NextObject**  hace lo mismo, pero navega al siguiente objeto de la colección.

**Vea también:**  [RecordsNavigationController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.RecordsNavigationController)  |  [RecordsNavigationController.PreviousObjectAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.RecordsNavigationController.PreviousObjectAction)  |  [RecordsNavigationController.NextObjectAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.RecordsNavigationController.NextObjectAction)

----------

### ShowNavigationItemController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **ShowNavigationItem**.

Activado en la ventana principal. La acción  **ShowNavigationItem**  permite a los usuarios finales navegar entre vistas predefinidas. En una aplicación de Windows Forms, esta acción se muestra en la barra de navegación. En una aplicación ASP.NET de formularios Web Forms, se muestra mediante fichas de navegación. Las vistas a las que puede navegar mediante esta acción se especifican en el nodo  **NavigationItems**  del modelo de aplicación.

**Vea también:**  [ShowNavigationItemController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ShowNavigationItemController)  |  [ShowNavigationItemController.ShowNavigationItemAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ShowNavigationItemController.ShowNavigationItemAction)  |  [IModelNavigationItems](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelNavigationItems)  |  [Sistema de navegación](https://docs.devexpress.com/eXpressAppFramework/113198/application-shell-and-base-infrastructure/navigation-system)

----------

### ViewNavigationController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **NavigateBack, NavigateForward**.

Activado en todas las ventanas. Las acciones implementadas por este controlador permiten a los usuarios finales navegar a las vistas invocadas recientemente. Estas acciones se activan solo en la ventana principal.

----------

### WebRecordsNavigationController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: heredadas.

El controlador se hereda de  [RecordsNavigationController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#recordsnavigationcontroller). Activa las acciones  **NextObject**  y  **PreviousObject**  si la vista actual es una vista detallada.


----------

### WebShowStartupNavigationItemController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

Activado en la ventana principal. Muestra una vista cuando se muestra la ventana de inicio. Utiliza  [ShowNavigationItemController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#shownavigationitemcontroller)  para obtener un elemento de navegación de inicio y ejecutar la acción  **ShowNavigationItem**.

----------

### WebViewNavigationController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones:  **NavegarA**  y heredado.

El controlador se hereda de  [ViewNavigationController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#viewnavigationcontroller). Desactiva las acciones heredadas  **NavigateBack**  y  **NavigateForward**. La acción  **NavigateTo**  se muestra como ruta de navegación y reemplaza la funcionalidad de acciones desactivadas.

----------

### WinShowStartupNavigationItemController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Activado en la ventana principal. Muestra una vista cuando se muestra la ventana de inicio. Utiliza  [ShowNavigationItemController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#shownavigationitemcontroller)  para obtener un elemento de navegación de inicio y ejecutar la acción  **ShowNavigationItem**.

----------

### WinViewNavigationController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: heredadas.

La versión específica de formularios Windows  [Forms de ViewNavigationController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#viewnavigationcontroller).

----------

## Vistas de lista

### ASPxGridListEditorPreviewRowViewController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista. El controlador se hereda del controlador  [ListEditorPreviewRowViewController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#listeditorpreviewrowviewcontroller). Inicializa la sección de vista previa, si la vista de lista utiliza  [ASPxGridListEditor](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.Editors.ASPx.ASPxGridListEditor).

----------

### AutoFilterRowListViewController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista.

Amplía la interfaz IModelClass del modelo de aplicación con la interfaz IModelClassShowAutoFilterRow y la interfaz IModelListView con la interfaz  [IModelListViewShowAutoFilterRow](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelClassShowAutoFilterRow).[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelClass)[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelListView)[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelListViewShowAutoFilterRow)

**Vea también:**  [Conceptos básicos del modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)  |  [Estructura del modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)  |  [Ampliar y personalizar el modelo de aplicación en código](https://docs.devexpress.com/eXpressAppFramework/112810/ui-construction/application-model-ui-settings-storage/customize-application-model-in-code/access-the-application-model-in-code)

----------

### CallbackStartupScriptController
Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

Activado en todas las páginas. Registra los scripts de inicio requeridos por los editores de listas en las devoluciones de llamada.

----------

### ColumnChooserControllerBase
Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

El controlador base para  [GridEditorColumnChooserController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#grideditorcolumnchoosercontroller)  y  [WinLayoutManagerController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#winlayoutmanagercontroller). Agrega los botones  **Agregar**  y  **Quitar**  a  **ColumnChooser**  o  **FieldList**  de una cuadrícula. Al presionar el botón  **Agregar**, el controlador muestra un árbol que representa las propiedades actuales del objeto.

----------

### GridEditorColumnChooserController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Un descendiente  [de ColumnChooserControllerBase](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#columnchoosercontrollerbase). Diseñado para vistas de lista que se muestran con  [GridListEditor](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.Editors.GridListEditor), que utiliza el editor  **XtraGrid**. El controlador configura el formulario de  **personalización**  del editor y admite su funcionalidad. Este formulario se puede invocar seleccionando el  **Selector de columnas**  en el menú contextual de la cuadrícula.

----------

### GridListEditorController

Plataforma: WindowsForms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista. Configura  [GridListEditor](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.Editors.GridListEditor)  si muestra la vista de lista actual. Destinado para uso interno.

----------

### GridListEditorPreviewRowController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista. Un descendiente  [de ListEditorPreviewRowViewController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#listeditorpreviewrowviewcontroller). Inicializa la sección de vista previa, si la vista de lista utiliza  [GridListEditor](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.Editors.GridListEditor).

----------

### ListEditorNewObjectController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista. Administra la creación de nuevos objetos mediante  [editores de listas](https://docs.devexpress.com/eXpressAppFramework/113189/ui-construction/list-editors)  mediante el control de los eventos ListEditor.NewObjectAdding, ListEditor.NewObjectCreated y  [ListEditor.NewObjectCanceled](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Editors.ListEditor.NewObjectCanceled)[.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Editors.ListEditor.NewObjectAdding)[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Editors.ListEditor.NewObjectCreated)

**Vea también:**  [NewObjectViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController)

----------

### ListEditorInplaceEditController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista. Guarda los cambios realizados mediante los editores locales del Editor de listas en la base de datos cuando la vista actual es la raíz o la estrategia de visualización actual funciona en modo  [ViewEditMode.View.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Editors.ViewEditMode)  Los cambios no se confirman automáticamente en las vistas de lista anidadas. Puede heredar este control y reemplazar la propiedad  **AutoCommitChanges**  para devolver  **true**.

**Ver también:**  [Editores de listas](https://docs.devexpress.com/eXpressAppFramework/113189/ui-construction/list-editors)  |  [ShowViewStrategy.CollectionsEditMode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.ShowViewStrategy.CollectionsEditMode)

----------

### ListEditorPreviewRowViewController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista. Extiende el  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)  con la propiedad  **PreviewColumnName**  de  **Views**  |  **_<ListView>_**  nodo. Sirve como clase base para los controladores que activan filas de vista previa en editores de listas específicos de la plataforma. Hay tres descendientes específicos de la plataforma: , GridListEditorPreviewRowController y  [ASPxGridListEditorPreviewRowViewController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#aspxgridlisteditorpreviewrowviewcontroller).  [](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#gridlisteditorpreviewrowcontroller) `DxGridListEditorPreviewRowController`

**Vea también:**  [IModelListViewPreviewColumn.PreviewColumnName](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelListViewPreviewColumn.PreviewColumnName)  |  [IModelListView](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelListView)

----------

### ListViewController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones:  **Editar**.

Activado para la vista de lista. La acción de edición proporcionada por este controlador invoca una vista detallada para el objeto que está seleccionado actualmente en una vista  **de**  lista. Esta vista detallada se muestra en modo de edición.

----------

### ListViewFocusedElementToClipboardController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones:  **CopyCellValue**.

Activado para vistas de lista. La acción  **CopyCellValue**, implementada por este controlador, permite copiar el contenido de la celda del Editor de listas enfocada en el portapapeles. El Editor de listas debe admitir la interfaz  [IFocusedElementCaptionProvider](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.Editors.IFocusedElementCaptionProvider).

----------

### ListViewProcessCurrentObjectController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **ListViewShowObject**.

Activado para vistas de lista. La acción  **ListViewShowObject**  se ejecuta al hacer doble clic en un objeto en una vista de lista en una aplicación de Windows Forms y al hacer clic en un objeto en una vista de lista en una aplicación de formularios Web Forms de ASP.NET. El objeto se muestra en una ventana independiente. Si necesita ejecutar una acción personalizada en lugar de la acción  **ListViewShowObject**, desactive este controlador y suscríbase al evento  [ListView.ProcessSelectedItem](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ListView.ProcessSelectedItem)  en un control personalizado.

**Vea también:**  [ListViewProcessCurrentObjectController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ListViewProcessCurrentObjectController)  |  [ListViewProcessCurrentObjectController.ProcessCurrentObjectAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ListViewProcessCurrentObjectController.ProcessCurrentObjectAction)  |  [Cómo: Reemplazar la acción predeterminada de una vista de lista](https://docs.devexpress.com/eXpressAppFramework/112820/ui-construction/controllers-and-actions/actions/how-to-replace-a-list-views-default-action)

----------

### NewItemRowDataSourcePropertyController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista. Para uso interno. Permite a los usuarios finales utilizar los editores de búsqueda de la fila Nuevo elemento de  [GridListEditor](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.Editors.GridListEditor).

----------

### NewItemRowListViewController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista. Amplía el  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)  con la propiedad  **DefaultListViewNewItemRowPosition**  en  **BOModel**  |  **_<Class>_**  y la propiedad  **NewItemRowPosition**  en  **Views**  |  **_<ListView>_**  nodo. Configura la nueva fila de elementos, si el Editor de listas implementa la interfaz  [ISupportNewItemRowPosition](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ISupportNewItemRowPosition).

**Vea también:**  [IModelClassNewItemRow.DefaultListViewNewItemRowPosition](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelClassNewItemRow.DefaultListViewNewItemRowPosition)  |  [IModelListViewNewItemRow.NewItemRowPosition](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelListViewNewItemRow.NewItemRowPosition)

----------

### ToolbarVisibilityController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones:  **ToggleToolbarVisibility**.

Activado para vistas de lista anidadas. La acción  **ToggleToolbarVisibility**  permite a los usuarios finales ocultar la barra de herramientas que acompaña a una vista de lista anidada. Para acceder a esta acción, haga clic con el botón derecho en una vista de lista anidada y seleccione  **Alternar barra de herramientas**  en el menú contextual invocado.

----------

### UpdateListEditorSelectedObjectsController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

Activado en vistas de lista. Actualiza la selección de un editor de listas cuando cambia la colección enlazada.

----------

### WebListEditorRefreshController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

Activado en vistas de lista. Actualiza un editor de listas cuando los datos mostrados por él se filtran o cambian.

**Vea también:**  [ListEditor.Refresh](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Editors.ListEditor.Refresh)  |  [Filtrado](https://docs.devexpress.com/eXpressAppFramework/112998/filtering)

----------

### WebListEditorSettingsStoreViewController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista. Agrega la propiedad SaveListViewStateInCookies al nodo  **Opciones**  del modelo de aplicación y la propiedad  **SaveStateInCookies**  a  **Views**  |  **Nodo Vista de lista**.  **SaveListViewStateInCookies**  es una configuración global para todas las vistas de lista.  **SaveStateInCookies**  es útil para establecer un valor individual para una vista de lista determinada. Cuando la configuración actual de la vista de lista debe guardarse en cookies, el controlador guarda las diferencias que se encuentran entre el estado de la vista de lista definido en el modelo de aplicación y el estado actual en la sesión actual. A continuación,  [SessionDictionaryDifferenceStoreWindowController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#sessiondictionarydifferencestorewindowcontroller)  guardará las diferencias de la sesión en cookies.

**Vea también:**  [IModelOptionsStateStore.SaveListViewStateInCookies](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.IModelOptionsStateStore.SaveListViewStateInCookies)  |  [IModelListViewStateStore.SaveStateInCookies](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.IModelListViewStateStore.SaveStateInCookies)

----------

## Seguridad

### LogoffController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **Cerrar sesión**.

Activado para todas las vistas. La acción de cierre de sesión permite a los usuarios finales iniciar sesión en la aplicación con otra cuenta  **de**  usuario. Esta acción está disponible cuando se usa la estrategia de autenticación estándar y se desactiva cuando se usa la estrategia de autenticación de Active Directory.

**Ver también:**  [LogoffController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.LogoffController)  |  [LogoffController.LogoffAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.LogoffController.LogoffAction)  |  [Sistema de seguridad](https://docs.devexpress.com/eXpressAppFramework/113366/data-security-and-safety/security-system)

----------

### LogonController
Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: heredadas.

Un descendiente de  [DialogController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#dialogcontroller). Activado en la ventana de inicio de sesión. Reemplaza la acción  **DialogOk**  del  **DialogController**  por la acción  **de inicio de sesión**. El controlador de eventos  **Execute**  de la acción lo proporciona la clase  **base DialogController**.

**Ver también:**  [Sistema de seguridad](https://docs.devexpress.com/eXpressAppFramework/113366/data-security-and-safety/security-system)

----------

### WebLogonController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: heredadas.

El controlador se hereda del controlador de diálogo  [LogonController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#logoncontroller). Desactiva la acción  **Cancelar**.

----------

## Dashboards

### DashboardCreationWizardController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **CreateDashboard**.

Activado en la ventana principal. Proporciona la acción  **CreateDashboard**, que permite a los usuarios finales crear paneles.

**Vea también:**  [IModelOptionsDashboard.EnableCreation](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelOptionsDashboard.EnableCreation)  |  [DashboardView](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.DashboardView)

----------

### DashboardCustomizationController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **OrganizeDashboard**.

Activado en vistas de detalles anidadas de  **DashboardOrganizer**. Proporciona la acción  **OrganizeDashboard**, que permite a los usuarios finales organizar paneles.

**Vea también:**  [IModelOptionsDashboard.EnableCustomization](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelOptionsDashboard.EnableCustomization)  |  [DashboardView](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.DashboardView)

----------

### DashboardDeactivateItemsActionsController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **OrganizeDashboard**.

Activado en las vistas del panel. Desactiva las acciones  **SaveAndClose**  y  **SaveAndNew**  en las vistas mostradas en un panel.

**Vea también:**  [ModificationsController.SaveAndCloseAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ModificationsController.SaveAndCloseAction)  |  [ModificacionesControlador.GuardarAndNuevoAcción](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ModificationsController.SaveAndNewAction)  |[DashboardView](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.DashboardView)

----------

### DashboardOrganizerHideToolbarController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Activado en vistas  **DashboardOrganizationItem**  anidadas. Deshabilita la barra de herramientas Acciones en el organizador del panel.

**Vea también:**  [IModelOptionsDashboard.EnableCustomization](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelOptionsDashboard.EnableCustomization)  |  [DashboardView](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.DashboardView)

----------

### DashboardOrganizerItemsCollectionsController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **DeleteItem**,  **HideItemsFromDashboard**,  **ShowItemsOnDashboard**.

Activado en vistas  **DashboardOrganizationItem**  anidadas. Proporciona acciones que permiten a los usuarios finales mostrar, ocultar y eliminar elementos de vista de panel en una vista de panel.

**Vea también:**  [IModelOptionsDashboard.EnableCustomization](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelOptionsDashboard.EnableCustomization)  |  [DashboardView](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.DashboardView)  |  [DashboardViewItem](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Editors.DashboardViewItem)

----------

### DashboardWinLayoutManagerController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Activado en las vistas del panel. Permite a los usuarios finales agregar nuevas vistas y quitar vistas existentes de un panel con el cuadro de diálogo Personalización.

**Vea también:**  [IModelOptionsDashboard.EnableCustomization](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelOptionsDashboard.EnableCustomization)  |  [DashboardView](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.DashboardView)  |  [DashboardViewItem](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Editors.DashboardViewItem)

----------

### DeleteDashboardsController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **DeleteDashboard**.

Activado en la ventana principal. Proporciona la acción  **EliminarDashboard**  que se encuentra en la categoría  **Herramientas**. Esta acción invoca un cuadro de diálogo emergente con una lista de todas las vistas de panel definidas en las diferencias de modelo del usuario. En esta ventana emergente, puede seleccionar uno o más paneles y hacer clic en  **Eliminar**  para eliminarlos. La propiedad  **DeleteDashboardsController.CanDeleteParentGroup**  especifica si el grupo primario del panel se elimina al quitar el último panel de este grupo. La acción  **DeleteDashboard**  está activa cuando hay paneles que se pueden eliminar y la propiedad  **EnableCreation**  de  **Options**  |  **El nodo Paneles**  se establece en  **true**  en el modelo de aplicación.

**Vea también:**  [IModelOptionsDashboard.EnableCustomization](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelOptionsDashboard.EnableCustomization)  |  [DashboardView](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.DashboardView)

----------

### ViewDashboardOrganizationItemController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Activado en  **las vistas ViewDashboardOrganizationItem**. Actualiza el organizador de dasboard cuando cambia un tipo de elemento de vista de dasboard.

**Vea también:**  [IModelOptionsDashboard.EnableCustomization](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelOptionsDashboard.EnableCustomization)  |  [DashboardView](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.DashboardView)

----------

## Depuración y pruebas

### ActionsInfoController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Un controlador de diagnóstico que implementa la interfaz  **IDiagnosticController**. Este Controlador recopila información sobre todos los Controladores del  [Marco (Ventana](https://docs.devexpress.com/eXpressAppFramework/112608/ui-construction/windows-and-frames)) actual, las Acciones de los Controladores, la  [Plantilla](https://docs.devexpress.com/eXpressAppFramework/112609/ui-construction/templates)  actual y sus  [Contenedores de Acciones](https://docs.devexpress.com/eXpressAppFramework/112610/ui-construction/action-containers), y la  [Vista](https://docs.devexpress.com/eXpressAppFramework/112611/ui-construction/views)  actual y su(s) Editor(es).  **DiagnosticInfoController**  agrega el elemento de acción de opción única expuesto por este controlador a la acción  **DiagnosticInfo**. Cuando un usuario final selecciona este elemento, la información recopilada se muestra en una ventana invocada.

----------

### DiagnosticInfoProviderBase
Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Activado en todas las ventanas y marcos.  **DiagnosticInfoProviderBase**  es la clase base para los controladores  [ActionsInfoController, ViewInfoController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#actionsinfocontroller)  y  [ShowRulesController](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#showrulescontroller).  [](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#viewinfocontroller)Estos controladores proporcionan elementos para la acción de  **información de diagnóstico**  de  [DiagnosticInfoController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#diagnosticinfocontroller). La clase  **DiagnosticInfoProviderBase**  expone miembros diseñados para crear los elementos de la acción de información  **de diagnóstico**  y recopilar la información mostrada al seleccionar estos elementos.

**Ver también:**  [Determinar por qué una acción, controlador o editor está inactivo](https://docs.devexpress.com/eXpressAppFramework/112818/ui-construction/controllers-and-actions/determine-why-an-action-controller-or-editor-is-inactive)  |  [ActionBase.DiagnosticInfo](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.DiagnosticInfo)

----------

### DiagnosticInfoController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **Información de diagnóstico**.

Activado en todas las ventanas y marcos. Busca todos los controladores que implementan la interfaz  **IDiagnosticController**. Estos Controladores recopilan información sobre los objetos requeridos (Controladores, Acciones, Plantilla actual, reglas de validación, etc.). La acción de información de diagnóstico de  **DiagnosticInfoController**, que representa una acción de opción única, contiene los elementos de acción de opción única expuestos por los controladores de  **diagnóstico**  que se encuentran. Cuando se selecciona el elemento Acción de información  **de diagnóstico**, se muestra una ventana con la información recopilada por el controlador correspondiente. Esta información ayuda a averiguar por qué el Controlador o la Acción están inactivos o invisibles.

**Ver también:**  [Determinar por qué una acción, controlador o editor está inactivo](https://docs.devexpress.com/eXpressAppFramework/112818/ui-construction/controllers-and-actions/determine-why-an-action-controller-or-editor-is-inactive)  |  [ActionBase.DiagnosticInfo](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.DiagnosticInfo)

----------

### LookupControlFinderController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Activado para las vistas mostradas por  **LookupPropertyEditor**. Detecta controles en ventanas emergentes y los proporciona a  [EasyTest](https://docs.devexpress.com/eXpressAppFramework/113211/debugging-testing-and-error-handling/functional-tests-easy-test).

**Ver también:**  [pruebas funcionales](https://docs.devexpress.com/eXpressAppFramework/113211/debugging-testing-and-error-handling/functional-tests-easy-test)

----------

### TestScriptsController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

Se activa en todas las ventanas cuando  [EasyTest](https://docs.devexpress.com/eXpressAppFramework/113211/debugging-testing-and-error-handling/functional-tests-easy-test)  está habilitado. Permite probar una aplicación XAF de formularios Web Forms ASP.NET con  **EasyTest**.

**Ver también:**  [pruebas funcionales](https://docs.devexpress.com/eXpressAppFramework/113211/debugging-testing-and-error-handling/functional-tests-easy-test)

----------

### ViewInfoController
Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Un controlador de diagnóstico que implementa la interfaz  **IDiagnosticController**. Este Controlador recopila información sobre la Vista actual y su(s) editor(es).  [DiagnosticInfoController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#diagnosticinfocontroller)  agrega el elemento de acción de opción única expuesto por este controlador a la acción  **DiagnosticInfo**. Cuando un usuario final selecciona este elemento, la información recopilada se muestra en una ventana invocada.

**Vea también:**  [Determinar por qué una acción, controlador o editor está inactivo](https://docs.devexpress.com/eXpressAppFramework/112818/ui-construction/controllers-and-actions/determine-why-an-action-controller-or-editor-is-inactive)

----------

### WindowControlFinderController
Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones:  **Control EasyTest**.

Activado en todas las ventanas. Detecta controles en la ventana actual y los proporciona a  [EasyTest](https://docs.devexpress.com/eXpressAppFramework/113211/debugging-testing-and-error-handling/functional-tests-easy-test). Declara la acción de diagnóstico  **EasyTest Control**, que enumera todos estos controles.

**Ver también:**  [pruebas funcionales](https://docs.devexpress.com/eXpressAppFramework/113211/debugging-testing-and-error-handling/functional-tests-easy-test)

----------

## Misceláneo

### AboutInfoController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones:  **AboutInfo**.

Activado en la ventana principal. Contiene la acción  **Acerca de la información**. En las aplicaciones de Windows Forms, esta acción presenta información general sobre la aplicación actual recopilada del modelo de aplicación por este controlador.

**Vea también:**  [Personalización de aplicaciones](https://docs.devexpress.com/eXpressAppFramework/113445/application-shell-and-base-infrastructure/application-personalization)

----------

### AboutInfoFormController
Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Activado en las vistas de detalle de los objetos  **AboutInfo**. Configura la ventana Acerca de:

-   Deshabilita la capacidad de personalizar diseños.
-   Corrige el ancho de la ventana según el ancho de la imagen del logotipo.
-   Quita el  [elemento Vista](https://docs.devexpress.com/eXpressAppFramework/112612/ui-construction/view-items-and-property-editors)  de imagen estática si no se especifica la imagen.
-   Permite que la característica  [Énfasis de etiqueta](https://docs.devexpress.com/eXpressAppFramework/113130/ui-construction/view-items-and-property-editors/apply-html-formatting-to-windows-forms-xaf-ui-elements)  divida el texto Acerca de en varias cadenas.

----------

### ActionsCriteriaViewController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Deshabilita y habilita Actions en función de sus valores de propiedad ActionBase.TargetObjectsCriteria y  [ActionBase.TargetObjectsCriteriaMode.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.TargetObjectsCriteria)[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.TargetObjectsCriteriaMode)

----------

### AsyncLoadingCancelationController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Cancela la  [carga asincrónica](https://docs.devexpress.com/eXpressAppFramework/401747/ui-construction/views/asynchronous-data-loading/asynchronous-data-loading)  de datos en una vista cuando un usuario la cierra. Este controlador le permite mostrar un mensaje de confirmación al cerrar una vista en la interfaz de usuario.

**Vea también**:  [Cómo: Personalizar el comportamiento asincrónico de carga de datos y la interfaz de usuario](https://docs.devexpress.com/eXpressAppFramework/401750/ui-construction/views/asynchronous-data-loading/how-to-customize-asynchronous-data-loading-behavior-and-ui)

----------

### AsyncLoadingIndicationController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Muestra el  [formulario de superposición](https://docs.devexpress.com/eXpressAppFramework/112680/application-shell-and-base-infrastructure/splash-forms)  y deshabilita las acciones integradas mientras hay una operación asincrónica en curso.

**Vea también**:  [Cómo: Personalizar el comportamiento asincrónico de carga de datos y la interfaz de usuario](https://docs.devexpress.com/eXpressAppFramework/401750/ui-construction/views/asynchronous-data-loading/how-to-customize-asynchronous-data-loading-behavior-and-ui)

----------

### ChooseSkinController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones:  **ChooseSkin**.

Se activa en la ventana principal si la propiedad  [UseLightStyle](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.WinApplication.UseLightStyle)  es  **false**. De lo contrario,  [ConfigureSkinController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#configureskincontroller)  se activa en su lugar.

Contiene la acción  **ChooseSkin**. Esta acción permite a los usuarios finales aplicar una máscara predefinida a la aplicación. La máscara seleccionada se almacena en la propiedad  [Skin](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.SystemModule.IModelApplicationOptionsSkin.Skin)  del nodo  **Opciones**  del modelo de aplicación.

**Vea también:**  [Conceptos básicos del modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)  |  [Estructura del modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)

----------

### ConfigureSkinController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones:  **ConfigureSkin**.

Se activa en la ventana principal si la propiedad  [UseLightStyle](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.WinApplication.UseLightStyle)  es  **true**. De lo contrario,  [ChooseSkinController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#chooseskincontroller)  se activa en su lugar.

Contiene la acción  **ConfigureSkin**. Esta acción permite a los usuarios finales aplicar una máscara predefinida a la aplicación. Los usuarios finales también pueden seleccionar una paleta si una máscara admite paletas. La máscara seleccionada se almacena en la propiedad  [Skin](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.SystemModule.IModelApplicationOptionsSkin.Skin)  del nodo  **Opciones**  del modelo de aplicación. La paleta seleccionada se almacena en la propiedad  [Palette](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.SystemModule.IModelApplicationOptionsPalette.Palette)  del nodo  **Opciones**  del modelo de aplicación.

**Vea también:**  [Conceptos básicos del modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)  |  [Estructura del modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)

----------

### ChooseThemeController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones:  **ChooseTheme**.

Contiene la acción  **ChooseTheme**. Esta acción permite a los usuarios finales seleccionar uno de los temas utilizados en la aplicación. El tema seleccionado actualmente se almacena en las cookies del navegador del usuario.

**Ver también:**  [ChooseThemeController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.SystemModule.ChooseThemeController)  |  [ASP.NET Apariencia de la aplicación de formularios Web Forms](https://docs.devexpress.com/eXpressAppFramework/113153/application-shell-and-base-infrastructure/themes/asp-net-web-application-appearance)

----------

### CloseMdiChildWindowController
Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win23.1.dll._  Acciones: ninguna.

Se activa en Windows secundario cuando se utiliza  [MdiShowViewStrategy](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.MdiShowViewStrategy). Prohíbe cerrar una ventana secundaria mientras su vista está cambiando y cierra la ventana cuando su vista está cerrada.

----------

### CloseWindowController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win23.1.dll._  Acciones:  **Cerrar**.

Activado en Windows infantil. La acción  **Cerrar**  cierra la ventana actual. Está activo cuando una ventana actual contiene una vista.

----------

### DetailViewEditorActionController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Activado para vistas detalladas. Recopila las acciones de los elementos de la vista detallada actual que implementan la interfaz  **IActionSource**  y garantiza que las acciones se activarán y se pueden ejecutar. La propiedad  **IActionSource.Actions**  expone las acciones.

----------

### DetailViewLinkController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Activado para vistas detalladas. Cuando se invoca una vista de detalle desde una vista de lista en una ventana independiente, este controlador actualiza la vista de lista si algo cambia en la vista de detalles. Por ejemplo, cuando el objeto de una vista de detalle se reemplaza por otro (por ejemplo, mediante la acción  **NextObject**), el objeto enfocado en la vista de lista cambia. Cuando se modifica o elimina un objeto en una vista de detalle, también se actualizan los objetos de la vista de lista. Cuando se abre un objeto en una vista de detalles desde una vista de lista anidada, todos los objetos nuevos creados en esta vista de detalles se vinculan automáticamente al objeto maestro, que posee la colección enlazada a la vista de lista anidada.  **DetailViewLinkController**  se suscribe al evento  [IObjectSpace.Committed.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.IObjectSpace.Committed)  En este controlador de eventos, se busca un objeto que coincida con  [DetailView.CurrentObject](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.DetailView.CurrentObject)  en la vista de lista desde la que se abrió la vista de detalles actual. El objeto encontrado se vuelve a cargar si se modifica  **DetailView.CurrentObject.**

----------

### DialogController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **DialogOK,**  **DialogCancel.**

Activado en ventanas emergentes invocadas por  [PopupWindowShowAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.PopupWindowShowAction). Contiene las acciones  **DialogOK**  y  **DialogCancel.**  Puede agregar este controlador a una ventana emergente al crearlo utilizando el objeto  [ShowViewParameters](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ShowViewParameters)  de una acción.

**Ver también:**  [DialogController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.DialogController)  |  [DialogController.AcceptAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.DialogController.AcceptAction)  |  [DialogController.CancelAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.DialogController.CancelAction)  |  [Controlador de diálogo](https://docs.devexpress.com/eXpressAppFramework/112805/ui-construction/controllers-and-actions/dialog-controller)

----------

### DockPanelsVisibilityController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Activado para todas las ventanas. Crea un  [SingleChoiceAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.SingleChoiceAction)  en la categoría  **Paneles**  para cada panel acoplable ubicado en la ventana actual si la  [plantilla](https://docs.devexpress.com/eXpressAppFramework/112609/ui-construction/templates)  de la ventana actual implementa la interfaz  **IDockManagerHolder**. Las acciones creadas le permiten cambiar la visibilidad del panel dock a  **Visible**,  **Ocultar automáticamente**  u  **Oculto**.

**Vea también:**  [Personalización de plantillas](https://docs.devexpress.com/eXpressAppFramework/112696/ui-construction/templates/template-customization)

----------
### EditModelController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones:  **EditModel**.

Activado en todas las ventanas. Contiene la acción  **EditarModelo**. Esta acción permite a los usuarios finales invocar el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor)  en tiempo de ejecución. Cuando se utiliza el  [sistema de seguridad](https://docs.devexpress.com/eXpressAppFramework/113366/data-security-and-safety/security-system), esta acción se desactiva si el usuario actual no tiene permiso para editar el modelo de aplicación.

----------

### ExitController
Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones:  **Salir**.

Contiene la acción  **de salida**, que se utiliza para cerrar la aplicación.

----------

### ExportAnalysisController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **Exportación**.

Se activa en las vistas detalladas cuando la colección  [CompositeView.Items](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.CompositeView.Items)  incluye editores que admiten la interfaz  [IExportableAnalysisEditor](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IExportableAnalysisEditor). El controlador base abstracto para los controladores WinExportAnalysisController y  **WebExportAnalysisController**.  Contiene la acción  **ExportAnalysis**  que exporta datos de los editores de propiedades de análisis (vea  [Módulo de gráfico dinámico](https://docs.devexpress.com/eXpressAppFramework/113024/analytics/pivot-chart-module)).

**Ver también:**  [ExportController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ExportController)  |  [ExportController.ExportAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ExportController.ExportAction)  |  [Cómo: Personalizar el comportamiento de la acción de exportación](https://docs.devexpress.com/eXpressAppFramework/113287/shape-export-print-data/printing-exporting-in-listview/how-to-customize-the-export-action-behavior)

----------

### ExportController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **Exportación**.

Activado en todas las vistas. El controlador base abstracto para los controladores  [WinExportController y WebExportController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#winexportcontroller).[](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#webexportcontroller)  Contiene la acción de exportación que exporta datos desde la vista  **de**  lista. La acción está activa en las vistas de lista cuando el editor  [ListView.Editor](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ListView.Editor)  admite la interfaz  [IExportable](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IExportable).

**Ver también:**  [ExportController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ExportController)  |  [ExportController.ExportAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ExportController.ExportAction)  |  [Cómo: Personalizar el comportamiento de la acción de exportación](https://docs.devexpress.com/eXpressAppFramework/113287/shape-export-print-data/printing-exporting-in-listview/how-to-customize-the-export-action-behavior)

----------

### FillActionContainersController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Cuando se crea una plantilla, también se crean todos sus contenedores de acciones. A continuación, el controlador  **FillActionContainers**  utiliza el  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)  para determinar las acciones que se van a mostrar en los contenedores de acciones. En particular, el  **ActionDesign**  |  **El nodo ActionToContainerMapping**  proporciona esta información. A continuación, este controlador llama al método  **Register**  del contenedor de acciones para cada acción para crear el control correspondiente. Los contenedores de acciones crean controles específicos para cada tipo de acción.

**Vea también:**  [IModelActionToContainerMapping](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelActionToContainerMapping)  |  [Acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  |  [Contenedores de acciones](https://docs.devexpress.com/eXpressAppFramework/112610/ui-construction/action-containers)

----------

### FocusDefaultDetailViewItemController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Amplía los nodos  **DetailView**  del  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)  con la propiedad  **DefaultFocusedItem**, que especifica el editor de propiedades inicialmente enfocado cuando se muestra la vista detallada raíz. Tenga en cuenta que la vista de detalle, que se muestra junto con la vista de lista (vea Mostrar una vista de  [detalles con una vista de lista](https://docs.devexpress.com/eXpressAppFramework/404203/getting-started/in-depth-tutorial-blazor/customize-data-display-and-view-layout/display-a-detail-view-with-a-list-view)) no es una raíz, por lo que la propiedad  **DefaultFocusedItem**  no tiene sentido en este caso. Los siguientes controladores específicos de la plataforma se derivan de FocusDefaultDetailViewItemController: WinFocusDefaultDetailViewItemController y  **WebFocusDefaultDetailViewItemController**.[](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#winfocusdefaultdetailviewitemcontroller)[](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#webfocusdefaultdetailviewitemcontroller)

**Vea también:**  [IModelDetailViewDefaultFocusedItem.DefaultFocusedItem](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelDetailViewDefaultFocusedItem.DefaultFocusedItem)  |  [IModelDetailView](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelDetailView)  |  [View.IsRoot](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.View.IsRoot)

----------

### FocusController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

Un controlador de ventana que administra el foco en la página actual.

**Ver también:**  [FocusController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.SystemModule.FocusController)

----------

### HideActionsViewController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Desactiva las acciones que se enumeran en el nodo secundario  **HiddenActions**  del nodo  **View**  de la vista actual.

**Ver también:**  [IModelHiddenActions](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelHiddenActions)  |  [IModelView](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelView)

----------

### HtmlFormattingController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Activado para todas las vistas. Agrega la propiedad  **EnableHtmlFormatting**  al nodo  **Options**. Cuando esta propiedad se establece en  **true**, se representa el formato HTML de los títulos del Editor de propiedades, los títulos de columna de la vista de lista y el texto del elemento de vista estática. Cuando esta propiedad se establece en  **false**, no se admite el formato HTML.

**Vea también:**  [IModelOptionsEnableHtmlFormatting.EnableHtmlFormatting](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.SystemModule.IModelOptionsEnableHtmlFormatting.EnableHtmlFormatting)  |  [IModelOptions](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelOptions)  |  [Cómo: Aplicar formato HTML a elementos de interfaz de usuario XAF de formularios Windows Forms](https://docs.devexpress.com/eXpressAppFramework/113130/ui-construction/view-items-and-property-editors/apply-html-formatting-to-windows-forms-xaf-ui-elements)

----------

### LockController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Activado para todas las vistas. Bloquea la modificación de un objeto, si se está modificando actualmente en otra vista.

**Ver también:**  [Control de concurrencia optimista](https://docs.devexpress.com/eXpressAppFramework/113596/business-model-design-orm/business-model-design-with-xpo/optimistic-concurrency-control)

----------

### MdiTabImageController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Se activa para  [Windows](https://docs.devexpress.com/eXpressAppFramework/112608/ui-construction/windows-and-frames)  secundario cuando se usa  [MdiShowViewStrategy](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.MdiShowViewStrategy)  en modo  [MdiMode.Tabbed.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.Templates.MdiMode)  Establece las imágenes de tabulación de acuerdo con las vistas mostradas.

**Ver también:**  [UIType.TabbedMDI](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.UIType)

----------

### ObjectMethodActionsViewController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: Acciones, añadidas con el atributo  **Acción**.

Activado para todas las vistas. Itera a través de todas las clases de negocio presentes en el nodo  **BOModel**  del modelo de aplicación, recopila métodos decorados con el atributo  **Action**  y los convierte en acciones simples. Cada acción se activa si el tipo de objeto de la vista actual corresponde a la clase a partir de la cual se generó.

**Véase también:**  [ActionAttribute](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.ActionAttribute)  |  [SimpleAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.SimpleAction)  |  [Agregar una acción simple mediante un atributo](https://docs.devexpress.com/eXpressAppFramework/402156/getting-started/in-depth-tutorial-blazor/add-actions-menu-commands/add-a-simple-action-using-an-attribute)  |  [Cómo: Crear una acción mediante el atributo Action](https://docs.devexpress.com/eXpressAppFramework/112619/ui-construction/controllers-and-actions/actions/how-to-create-an-action-using-the-action-attribute)

----------

### OpenObjectController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones:  **OpenObject**.

Activado para todas las vistas. La acción se ejecuta después de:

-   Enfocar un editor de propiedades de referencia y hacer clic en el botón Acción de la barra de herramientas.
-   Mantenga presionada la tecla MAYÚS+CTRL y haga clic en un editor de propiedades de referencia.
-   Enfocar un editor y presionar Mayús+Ctrl+Entrar.

En las vistas detalladas, este controlador funciona con editores de propiedades heredados de la clase  [WinPropertyEditor](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.Editors.WinPropertyEditor); si el editor actualmente enfocado está enlazado a una propiedad de referencia, la acción  **OpenObject**  invoca la vista de detalle del objeto al que se hace referencia. En las vistas de lista, este controlador funciona cuando el  [ListView.Editor](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ListView.Editor)  actual es  [GridListEditor](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.Editors.GridListEditor); la acción  **OpenObject**  invoca la vista detallada del objeto desde la celda enfocada.

>NOTA
>
>-   La acción  **OpenObject**  está  inactiva cuando la vista de lista está en los modos [DataView](https://docs.devexpress.com/eXpressAppFramework/113683/ui-construction/views/list-view-data-access-modes), [ServerView, InstantFeedback, and InstantFeedbackView](https://docs.devexpress.com/eXpressAppFramework/118450/ui-construction/views/list-view-data-access-modes/server-server-view-instant-feedback-and-instant-feedback-view-modes)(https://docs.devexpress.com/eXpressAppFramework/118450/ui-construction/views/list-view-data-access-modes/server-server-view-instant-feedback-and-instant-feedback-view-modes).[](https://docs.devexpress.com/eXpressAppFramework/113683/ui-construction/views/list-view-data-access-modes)
>-   La acción  **OpenObject** no funciona con una propiedad de referencia en [TreeListEditor](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.TreeListEditors.Win.TreeListEditor).
-   El [IModelMemberViewItem.View](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelMemberViewItem.View) se omite al invocar la vista Detalle desde la vista de lista mediante la combinación Ctrl+Mayús+Clic.

Para especificar la vista invocada en la ejecución de la acción OpenObject, use el evento  **OpenObjectController.CustomOpenObject**, como se muestra en  [Cómo especificar qué DetailView muestra la acción Abrir registro relacionado ejecutar o la combinación de teclas ctrl Mayús, haga clic en  Vale Centro](https://supportcenter.devexpress.com/ticket/details/t437813/how-to-specify-what-detailview-is-shown-by-the-open-related-record-action-execute-or)  de soporte.

----------

### ParameterlessLogonFailedInfoViewController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

Destinado para uso interno. Personaliza un mensaje de error que se muestra cuando se produce un error en un procedimiento de inicio de sesión sin parámetros.

----------

### PreserveValidationErrorMessageAfterPostbackController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

Activado en vistas de raíz. Evita que los mensajes de error de validación que se muestran en la pantalla se pierdan cuando se produce una devolución de datos.

**Ver también:**  [Módulo de validación](https://docs.devexpress.com/eXpressAppFramework/113684/validation-module)

----------

### PrintingController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones:  **PageSetup**,  **Print**  y  **PrintPreview**.

Activado para vistas detalladas cuyo control  [implementa la interfaz IPrintable](https://docs.devexpress.com/WindowsForms/DevExpress.XtraPrinting.IPrintable)  y vistas de lista cuyo editor de  [listas](https://docs.devexpress.com/eXpressAppFramework/113189/ui-construction/list-editors)  implementa la interfaz  [IExportable](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IExportable). Proporciona acciones que permiten a los usuarios finales imprimir la vista actual.

**Vea también:**  [PrintingController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.SystemModule.PrintingController)

----------

### ProcessActionContainerHolderController
Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

Representa una clase base de la que se derivan los controladores que personalizan los controles del contenedor de acciones.

----------

### RedirectOnCallbackController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

Activado en todas las páginas. Diseñado para uso interno. Cuando se cambia una vista en un  [marco](https://docs.devexpress.com/eXpressAppFramework/112608/ui-construction/windows-and-frames), este controlador realiza la redirección, si es necesario.

----------

### RedirectOnViewChangedController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

Activado para todas las vistas. Diseñado para uso interno. Cuando se cambia una vista en un  [fotograma](https://docs.devexpress.com/eXpressAppFramework/112608/ui-construction/windows-and-frames), se pueden reemplazar algunos controles. El controlador  **RedirectOnViewChangedController**  impide la carga de datos de estado de vista y formulario en el control incorrecto.

----------

### RefreshController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **Actualizar**.

Activado para todas las vistas. Contiene la acción  **Actualizar**, que se activa sólo para las vistas raíz. Al ejecutar esta acción, se llama al método  [BaseObjectSpace.Refresh.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.BaseObjectSpace.Refresh)

**Consulte también:**  [RefreshController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.RefreshController)  |  [RefreshController.RefreshAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.RefreshController.RefreshAction)

----------

### RegisterThemeAssemblyController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

Activado en todas las páginas. Registra archivos CSS relacionados con temas almacenados como recursos en  **DevExpress.Web.ASPxThemes.23.1.dll**  en la página actual. Si necesita usar archivos CSS personalizados, puede deshabilitar este  [Controller](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers)  estableciendo su propiedad  **EnableXafThemeAssembly**  en  **false**.

**Vea también:**  [ASP.NET apariencia de la aplicación de formularios Web Forms](https://docs.devexpress.com/eXpressAppFramework/113153/application-shell-and-base-infrastructure/themes/asp-net-web-application-appearance)

----------

### ResetViewSettingsController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones:  **ResetViewSettings**.

Activado para todas las vistas. Contiene la acción  **ResetViewSettings**, que vuelve a abrir la vista actual y restablece todas las personalizaciones de usuario del modelo de la vista. Si utiliza el modo de presentación  **ListViewAndDetailView**, la acción  **ResetViewSettings**  se aplica a las vistas de lista y de detalle. Esta acción está deshabilitada (atenuada) si hay cambios no guardados.

----------

### SessionDictionaryDifferenceStoreWindowController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

El Controlador guarda las diferencias del  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)  realizadas por un usuario final de la sesión actual en el almacenamiento subyacente (por ejemplo, en cookies) cuando se carga una página antes de la representación.

**Vea también:**  [IModelListViewStateStore.SaveStateInCookies](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.IModelListViewStateStore.SaveStateInCookies)  |  [IModelOptionsStateStore.SaveListViewStateInCookies](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.IModelOptionsStateStore.SaveListViewStateInCookies)

----------

### VersionsCompatibilityController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Activado en todas las vistas. Cuando se trabaja con una aplicación, se puede actualizar su versión y base de datos. Esto puede dañar la base de datos. Este controlador comprueba la compatibilidad de las versiones de la aplicación y la base de datos periódicamente durante la ejecución de la aplicación. Si se encuentra una discrepancia de versión, se invoca una ventana de advertencia.

----------

### WaitCursorController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Cambia el cursor al modo de reloj de arena mientras se ejecuta una acción.

----------

### WebExportController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: heredadas.

Un descendiente específico de formularios Web  [Forms de ASP.NET ExportController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#exportcontroller). Crea una secuencia de memoria para los datos exportados.

**Vea también:**  [WebExportController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.SystemModule.WebExportController)

----------

### WebFocusDefaultDetailViewItemController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

El descendiente específico de la plataforma de  [FocusDefaultDetailViewItemController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#focusdefaultdetailviewitemcontroller).

----------

### WebFocusPopupWindowController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

Activado en ventanas emergentes. Este controlador centra la acción parametrizada  **FullTextSearch**  cuando la ventana emergente invocada contiene una vista de lista.

**Ver también:**  [FilterController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.FilterController)  |  [FilterController.FullTextFilterAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.FilterController.FullTextFilterAction)

----------

### WebIdAssignationController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

Para uso interno. Activado para vistas detalladas. Establece identificadores para todos los elementos de vista.

----------

### WebObjectMethodActionsViewController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: heredadas.

El controlador se hereda de  [ObjectMethodActionsViewController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#objectmethodactionsviewcontroller). Confirma los cambios realizados por una acción creada mediante  [ActionAttribute](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.ActionAttribute)  cuando una vista detallada está en modo de vista.

----------

### WebWindowController
Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Web.v23.1.dll._  Acciones: ninguna.

Activado para todas las ventanas. Garantiza el procesamiento correcto de la transformación del objeto actual en una vista detallada. Para uso interno.

----------

### WindowTemplateController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Actualiza los mensajes de título y estado de la ventana actual. Expone eventos y métodos que permiten personalizar y actualizar los mensajes de subtítulos y estado. Tiene un descendiente específico de WinForms:  [WinWindowTemplateController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#winwindowtemplatecontroller).

**Vea también:**  [WindowTemplateController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.WindowTemplateController)  |  [Cómo: Personalizar un título de ventana](https://docs.devexpress.com/eXpressAppFramework/113252/application-shell-and-base-infrastructure/application-personalization/customize-a-window-caption)  |  [Cómo: Personalizar mensajes de estado de ventana (WinForms)](https://docs.devexpress.com/eXpressAppFramework/113253/ui-construction/templates/in-winforms/how-to-customize-window-status-messages-winforms)

----------

### WinExportController
Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

El descendiente específico de formularios Windows Forms de  [ExportController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#exportcontroller). Crea una secuencia de archivos para los datos exportados.

**Vea también:**  [WinExportController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.SystemModule.WinExportController)

----------

### WinFocusDefaultDetailViewItemController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

El descendiente específico de la plataforma de  [FocusDefaultDetailViewItemController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#focusdefaultdetailviewitemcontroller).

----------

### WinFocusListEditorControlController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Activado en todas las ventanas. Cuando la vista que se muestra actualmente cambia a una vista de lista, centra el control del editor de listas.

**Ver también:**  [Editores de listas](https://docs.devexpress.com/eXpressAppFramework/113189/ui-construction/list-editors)

----------

### WinLayoutManagerController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Configura el formulario de  **personalización**  del editor y admite su funcionalidad. Este formulario se puede invocar seleccionando  **Personalizar diseño**  en el menú contextual del formulario de detalles.

**Vea también:**  [Ver personalización del diseño de elementos](https://docs.devexpress.com/eXpressAppFramework/112817/ui-construction/views/layout/view-items-layout-customization)

----------

### WinWindowTemplateController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.v23.1.dll._  Acciones: ninguna.

Un descendiente específico de WinForms de  [WindowTemplateController](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module#windowtemplatecontroller). Controla los eventos de  [DocumentManager](https://docs.devexpress.com/WindowsForms/DevExpress.XtraBars.Docking2010.DocumentManager)  para actualizar el título de la ventana actual.

----------

### XtraGridInLookupController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Win.v23.1.dll._  Acciones: ninguna.

Activado para todas las vistas. Destinado para uso interno. Configura el control  **XtraGrid**  utilizado en la ventana de búsqueda del Editor de propiedades de búsqueda en las vistas Detalle y Lista.


# Controladores integrados y acciones en módulos adicionales

Los  [controladores](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers)  integrados y sus  [acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  suministradas con los  [módulos adicionales](https://docs.devexpress.com/eXpressAppFramework/118046/application-shell-and-base-infrastructure/application-solution-components/modules#modules-shipped-with-xaf)  de  **eXpressApp Framework**  se enumeran en este tema. Tenga en cuenta que puede encontrar más información sobre un controlador o acción en particular en el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor): navegue hasta  **ActionDesign**  |[](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)  **Controladores**  o  **ActionDesign**  |  **Nodo Acciones**. Para determinar si una acción se muestra o no en una ventana determinada, asegúrese de que el contenedor de acciones que muestra la acción está contenido en esta plantilla. Para determinar el contenedor de acciones de la acción, utilice  **ActionDesign**  del modelo de aplicación |  **Nodo ActionToContainerMapping**.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/8493287d-50f4-4079-8332-9353d01e5719)


## Módulo de seguimiento de auditoría

### AuditInformationReadonlyViewController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.AuditTrail.Xpo.v23.1.dll._  Acciones: ninguna.

Activado para vistas que muestran objetos  **IBaseAuditDataItemPersistent**. Hace que estas vistas sean de solo lectura. Se utiliza para prohibir a los usuarios finales editar la información de auditoría.

----------

### AuditInformationListViewRefreshController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.AuditTrail.Xpo.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista anidadas que muestran objetos  **IBaseAuditDataItemPersistent**. Haga que las nuevas entradas de auditoría sean inmediatamente accesibles, después de guardar los cambios correspondientes en un objeto.

----------

### AuditTrailListViewController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.AuditTrail.Xpo.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista. Utilizado internamente. Cuando se carga una colección con un criterio, el controlador cambia el tipo de auditoría a ObjectChanged (si el tipo ObjectLoaded se estableció anteriormente).

----------

### AuditTrailViewController
Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.AuditTrail.Xpo.v23.1.dll._  Acciones: ninguna.

Activado para las vistas de detalle de raíz. Activa el sistema de auditoría de cambios para la sesión XPO en la que se invoca una vista.

----------

## Módulo de clonación de objetos

### CloneObjectViewController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.CloneObject.Xpo.v23.1.dll._  Acciones: ninguna.

Activado para las vistas de lista y detalle. Contiene la acción  **de clonación**. Esta acción permite a un usuario final clonar un objeto seleccionado actualmente en una vista de lista o en el objeto actual de una vista de detalle. Invoca una vista detallada con un nuevo objeto. Los valores de propiedad de este objeto se copian del objeto que se ha clonado.

**Vea también:**  [CloneObjectViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.CloneObject.CloneObjectViewController)

----------

## Módulo de apariencia condicional

### ActionAppearanceController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ConditionalAppearance.v23.1.dll._  Acciones: ninguna.

Activado para las vistas de lista y detalle. Recopila las acciones, mencionadas en las reglas de apariencia del objeto actual, y actualiza su apariencia.

----------

### AppearanceController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ConditionalAppearance.v23.1.dll._  Acciones: ninguna.

Activado para las vistas de lista y detalle. Recopila las reglas de apariencia actuales declaradas para la clase de negocio (vea  [IModelConditionalAppearance](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ConditionalAppearance.IModelConditionalAppearance)  y  [AppearanceAttribute](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ConditionalAppearance.AppearanceAttribute)). Expone el método  **RefreshItemAppearance**  utilizado por otros controladores de módulo para actualizar la apariencia.

**Ver también:**  [AppearanceController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ConditionalAppearance.AppearanceController)

----------

### DetailViewItemAppearanceController
Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ConditionalAppearance.v23.1.dll._  Acciones: ninguna.

Activado para vistas detalladas. Actualiza la apariencia de los elementos de la vista de detalles.

----------

### DetailViewLayoutItemAppearanceController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ConditionalAppearance.v23.1.dll._  Acciones: ninguna.

Activado para vistas detalladas. Actualiza la apariencia de los elementos de diseño.

----------

### ListViewItemAppearanceController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ConditionalAppearance.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista. Actualiza la apariencia actual de  [ListEditor](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Editors.ListEditor), si el editor admite la interfaz  **ISupportAppearanceCustomization**.

----------

### RefreshAppearanceController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ConditionalAppearance.v23.1.dll._  Acciones: ninguna.

Activado para las vistas de lista y detalle. Actualiza la apariencia cuando se producen los eventos  [View.SelectionChanged](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.View.SelectionChanged), View.CurrentObjectChanged, BaseObjectSpace.ObjectChanged y  [BaseObjectSpace.Committed](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.BaseObjectSpace.Committed)[.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.View.CurrentObjectChanged)[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.BaseObjectSpace.ObjectChanged)

----------

### RefreshItemsAppearanceControllerBase<T>

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ConditionalAppearance.v23.1.dll._  Acciones: ninguna.

La clase base abstracta de DetailViewItemAppearanceController y  [DetailViewLayoutItemAppearanceController](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#detailviewitemappearancecontroller).[](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#detailviewlayoutitemappearancecontroller)

----------

## Módulo de paneles

### DashboardNavigationController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Dashboards.v23.1.dll._

Asigna un  [acceso directo](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewShortcut)  a la vista de detalle del panel para cada elemento de navegación compatible con  [la interfaz IModelDashboardNavigationItem](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Dashboards.IModelDashboardNavigationItem).

----------

### HideDetailViewActionsController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Dashboards.v23.1.dll._

Activado para la vista detallada "DashboardViewer_DetailView". Desactiva NewObjectViewController,  [ModificationsController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ModificationsController)  y  [DeleteObjectsViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.DeleteObjectsViewController)  para prohibir la creación, edición y eliminación manual de instancias de panel.[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController)

----------

### NewDashboardController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Dashboards.v23.1.dll._

La clase base de WinNewDashboardController y  [WebNewDashboardController](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#winnewdashboardcontroller).[](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#webnewdashboardcontroller)  Implementa la funcionalidad común necesaria para crear nuevos paneles.

----------

### WebDashboardActionsController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Dashboards.Web.v23.1.dll._  Acciones:  **ExportDashboardAction, DashboardParametersAction**.

Activado para la vista detallada  [de IDashboardData](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.IDashboardData)  con el identificador "DashboardViewer_DetailView". La acción  **ExportDashboardAction**  permite a los usuarios finales exportar los paneles a varios formatos. La acción  **DashboardParametersAction**  permite a los usuarios finales invocar el cuadro de diálogo  [Parámetros del panel](https://docs.devexpress.com/Dashboard/117549/web-dashboard/create-dashboards-on-the-web/data-analysis/dashboard-parameters/specify-dashboard-parameter-values).

----------

### WebDashboardDesignerPopupController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Dashboards.Web.v23.1.dll._

Activado para la vista detallada "DashboardViewer_DetailView". Contiene ASP.NET código específico de formularios Web Forms que realiza la instalación del control  [ASPxDashboard](https://docs.devexpress.com/Dashboard/DevExpress.DashboardWeb.ASPxDashboard?v=23.1).

----------

### WebDashboardDialogController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Dashboards.Web.v23.1.dll._  Acciones:  **Aceptar**.

Un descendiente de  [DialogController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.DialogController)  activado para el cuadro de diálogo Diseñador de paneles. Introduce la acción de  **aceptación**  personalizada y desactiva la acción  **de cancelación**.

----------

### WebDashboardWindowController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Dashboards.Web.v23.1.dll._

Registra el script de cliente que realiza la configuración del control del panel del lado cliente.

----------

### WebEditDashboardController
Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Dashboards.Web.v23.1.dll._  Acciones:  **ShowDesignerAction**.

Activado en las vistas Lista y Detalle de los objetos  [IDashboardData](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.IDashboardData). Hace que la vista actual sea de solo lectura para prohibir la creación, edición y eliminación manual de instancias de panel.  **La acción ShowDesignerAction**  permite a los usuarios finales modificar paneles en el  [Diseñador de paneles](https://docs.devexpress.com/Dashboard/116518/basic-concepts-and-terminology/dashboard-designer).

----------

### WebNewDashboardController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Dashboards.Web.v23.1.dll._

Hereda el controlador  [NewDashboardController](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#newdashboardcontroller). Contiene ASP.NET código específico de formularios Web Forms, lo que proporciona a los usuarios finales la capacidad de crear nuevos paneles.

----------

### WinEditDashboardController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Dashboards.Win.v23.1.dll._  Acciones:  **ShowDesignerAction**.

Activado en las vistas Lista y Detalle de los objetos  [IDashboardData](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.IDashboardData).  **La acción ShowDesignerAction**  permite a los usuarios finales modificar paneles en el Diseñador de paneles. Utiliza  [WinShowDashboardDesignerController](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#winshowdashboarddesignercontroller)  para mostrar el  [Diseñador de paneles de WinForms](https://docs.devexpress.com/Dashboard/117006/winforms-dashboard/winforms-designer).

----------

### WinExportDashboardController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Dashboards.Win.v23.1.dll._  Acciones:  **ExportAction**.

Activado para la vista detallada "DashboardViewer_DetailView".  **La acción ExportAction**  permite a los usuarios finales exportar los paneles a varios formatos.

----------

### WinNewDashboardController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Dashboards.Win.v23.1.dll._

Hereda  [NewDashboardController](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#newdashboardcontroller). Contiene código específico de formularios Windows Forms, lo que proporciona a los usuarios finales la capacidad de crear nuevos paneles. Utiliza  [WinShowDashboardDesignerController](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#winshowdashboarddesignercontroller)  para mostrar el  [Diseñador de paneles de WinForms](https://docs.devexpress.com/Dashboard/117006/winforms-dashboard/winforms-designer).

----------

### WinPrintDashboardController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Dashboards.Win.v23.1.dll._  Acciones:  **PrintAction**.

Activado para la vista detallada "DashboardViewer_DetailView". La acción  **PrintAction**  permite a los usuarios finales imprimir el panel actual.

----------

### WinShowDashboardDesignerController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Dashboards.Win.v23.1.dll._

Contiene la instancia de  [DashboardDesignerManager](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Dashboards.Win.DashboardDesignerManager). Expone los métodos  **ShowDesigner**  y  **GetDashboardData**  que los controladores del módulo Dashboards utilizan para mostrar el  [Diseñador de paneles de WinForms](https://docs.devexpress.com/Dashboard/117006/winforms-dashboard/winforms-designer)  y, a continuación, obtener el objeto  [IDashboardData](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.IDashboardData)  modificado.

----------

## Módulo de archivos adjuntos

### FileAttachmentControllerBase

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.FileAttachment.Win.v23.1.dll._  Acciones:  **Abrir**,  **GuardarTo**.

La clase base que implementa la funcionalidad común de FileAttachmentController y  [FileAttachmentListViewController](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#fileattachmentcontroller.win).[](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#fileattachmentlistviewcontroller)

----------

### FileAttachmentController
Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.FileAttachment.Win.v23.1.dll._  Acciones:  **Abrir**,  **GuardarTo**.

Se activa cuando el tipo de objeto de la vista actual utiliza el atributo  **FileAttachment**.  **Open**  Action permite a los usuarios finales abrir un archivo mediante una aplicación asociada. La acción  **Guardar**  permite a los usuarios finales guardar un archivo en un disco local.

**Véase también:**[FileAttachmentController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.FileAttachments.Win.FileAttachmentController)

----------

### FileAttachmentController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.FileAttachment.Web.v23.1.dll._  Acciones:  **Descargar**.

Se activa cuando el tipo de objeto de la vista actual utiliza el atributo  **FileAttachment**. Contiene la acción de  **descarga**  que permite a los usuarios finales cargar el contenido de un archivo mediante un explorador.

**Véase también:**[FileAttachmentController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.FileAttachments.Web.FileAttachmentController)

----------

### FileAttachmentListViewController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.FileAttachment.Win.v23.1.dll._  Acciones:  **AddFromFile**.

Activado para vistas de lista, cuyo tipo de objeto utiliza el atributo  **FileAttachment**. Contiene la acción  **AddFromFile**  que permite a los usuarios finales agregar datos adjuntos de archivo a vistas de lista mediante un cuadro de diálogo estándar  **Abrir archivo**. Este controlador también permite agregar archivos adjuntos a una vista de lista mediante una operación de arrastrar y soltar.

**Vea también:**[FileAttachmentListViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.FileAttachments.Win.FileAttachmentListViewController)

----------

## Módulo KPI

### AutoRefreshKpiController
Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Kpi.v23.1.dll._  Acciones: ninguna.

El controlador se hereda de  [RefreshKpiController](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#refreshkpicontroller). Actualiza los objetos  **IKpiInstance**  después de volver a crear la colección Vista de lista del origen de colección.

----------

### DrilldownController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Kpi.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista anidadas que muestran objetos  **IKpiInstance**. El controlador suscribe el evento  [ActionBase.Running](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Executing)  de la acción  [ListViewProcessCurrentObjectController.ProcessCurrentObjectAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ListViewProcessCurrentObjectController.ProcessCurrentObjectAction)  y muestra la vista de lista detallada cuando un usuario final hace clic en un objeto  **IKpiInstance**. Si la vista de lista actual es una búsqueda, no se controla el evento  **Executioning**. La vista de lista con un identificador, especificado por la propiedad  **IKpiDefinition.DrillDownListViewId**, se utiliza como desglose de forma predeterminada. Puede controlar el evento  **DrilldownViewInitialized**  del controlador y especificar una vista de lista personalizada a través del parámetro  **ModelView**  del controlador.

----------

### ForceRefreshKpiController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Kpi.v23.1.dll._  Acciones:  **RefreshKpi**.

El controlador se hereda de  [RefreshKpiController](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#refreshkpicontroller). La acción  **RefreshKPI**  permite a los usuarios finales actualizar manualmente los objetos  **IKpiInstance**  mostrados.

----------

### KpiDashboardOrganizationItemController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Kpi.v23.1.dll._  Acciones: ninguna.

Activado para vistas detalladas que muestran objetos  **KpiDashboardOrganizationItem**. Para uso interno.

----------

### KpiDefinitionPreviewController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Kpi.v23.1.dll._  Acciones: ninguna.

Activado para vistas detalladas que muestran objetos  **IKpiDefinition**. Proporciona una vista previa (desglose) del objeto IKpiDefinition actual, que se actualiza cuando se cambia la propiedad  **IKpiDefinition.TargetObjectType.**  La vista de lista anidada con un identificador, especificada por la propiedad  **IKpiDefinition.DrillDownListViewId**, se utiliza para mostrar una vista previa.

----------

### KpiInstanceDeactivateNewDeleteViewController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Kpi.v23.1.dll._  Acciones: ninguna.

Activado para las vistas de lista y detalle que muestran objetos  **IKpiInstance**. Desactiva los controladores  [NewObjectViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController)  y  [DeleteObjectsViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.DeleteObjectsViewController)  para prohibir la creación y eliminación manual de instancias de KPI.

----------

### RefreshKpiController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Kpi.v23.1.dll._  Acciones: ninguna.

La clase base de AutoRefreshKpiController y  [ForceRefreshKpiController](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#autorefreshkpicontroller).[](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#forcerefreshkpicontroller)  Activado para las vistas de lista y detalle que muestran objetos  **IKpiInstance**. Implementa la funcionalidad común necesaria para actualizar las instancias de KPI.

----------

### ShowKpiAsChartController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Kpi.v23.1.dll._  Acciones:  **ShowChart**.

Activado para vistas de lista que muestran objetos  **IKpiInstance**. La acción  **MostrarGráfico**  invoca la ventana emergente con una vista  **de detalles KpiInstance_Chart_DetailView**. Esta vista proporciona la representación gráfica del objeto  **IKpiInstance**  seleccionado.

----------

## Módulo de Office

### MailMergeController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Office.Win.v23.1.dll._  Acciones: ninguna.

Activado para vistas que muestran objetos  **IRichTextMailMergeData**. Implementa la funcionalidad común de  [combinación de correspondencia](https://docs.devexpress.com/eXpressAppFramework/400006/document-management/office-module/mail-merge). Permite personalizar la configuración de Combinar correspondencia.

----------

### MenuManagerController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Office.Win.v23.1.dll._  Acciones: ninguna.

Activado para vistas que contienen  **RichTextPropertyEditor**. Crea un Administrador de menús para  **RichTextPropertyEditor**, en función de la configuración de la aplicación (vea  [Usar documentos de texto enriquecido en Business Objects](https://docs.devexpress.com/eXpressAppFramework/400004/document-management/office-module/use-rich-text-documents-in-business-objects)). Permite personalizar el menú de la cinta y las barras.

----------

### OfficeActionsController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Office.Win.v23.1.dll._  Acciones:  **OpenAction**,  **SaveAsAction**

Activado para vistas que contienen  **RichTextPropertyEditor o**  **SpreadsheetPropertyEditor**. Personaliza las acciones de impresión para el menú principal y el botón de la aplicación.  **OpenAction**  permite a los usuarios abrir un documento en diferentes formatos.  **SaveAsAction**  permite a los usuarios guardar documentos en el sistema de archivos en diferentes formatos.

----------

### RichTextGridController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Office.Win.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista. Aplica la configuración interna a  **RichEditControl**  antes de que  **GridView**  lo muestre en una celda. Permite personalizar el control.

----------

### RichTextServiceController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Office.Win.v23.1.dll._  Acciones: ninguna.

Permite personalizar  **RichEditControl**  y  **RichTextRibbonForm**.

----------

### ShowDocumentInPopupController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Office.Win.v23.1.dll._  Acciones: ninguna.

Activado para vistas que contienen  **RichTextPropertyEditor**. Agrega un elemento de menú contextual. Este elemento abre el contenido del editor de texto enriquecido actual en una ventana emergente. Permite personalizar  **RichTextRibbonForm**.

----------

### ShowInDocumentController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Office.Win.v23.1.dll._  Acciones:  **ShowInDocument**

Activado para vistas. La acción  **ShowInDocument**  se activa para las vistas cuyo tipo de objeto se utiliza en objetos de plantilla de combinación de correspondencia.

----------

### SpreadsheetMenuManagerController
Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Office.Win.v23.1.dll._  Acciones: ninguna.

Activado para vistas que contienen  **SpreadsheetPropertyEditor**. Crea un administrador de menús para  **SpreadsheetPropertyEditor**, en función de la configuración de la aplicación (consulte  [Usar documentos de hoja de cálculo en Business Objects](https://docs.devexpress.com/eXpressAppFramework/400931/document-management/office-module/use-spreadsheet-documents-in-business-objects). Permite personalizar el menú de la cinta y las barras.

----------

### SpreadsheetServiceController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Office.Win.v23.1.dll._  Acciones: ninguna.

Permite personalizar  **SpreadsheetControl**  y su  **RibbonControl**.

----------

### SpreadsheetShowDocumentInPopupController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Office.Win.v23.1.dll._  Acciones: ninguna.

Activado para vistas que contienen  **SpreadsheetPropertyEditor**. Agrega un elemento de menú contextual. Este elemento abre el contenido del editor de hojas de cálculo actual en una ventana emergente. Permite personalizar  **SpreadsheetRibbonForm**.

----------

### WebRichTextShowInDocumentController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Office.Web.v23.1.dll._  Acciones:  **ShowInDocument**

La acción  **ShowInDocument**  se activa para las vistas cuyo tipo de objeto se utiliza en objetos de plantilla de combinación de correspondencia.

----------

### WebRichTextMailMergeController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Office.Web.v23.1.dll._  Acciones: ninguna.

Activado para vistas que muestran objetos  **IRichTextMailMergeData**. Implementa la funcionalidad común de  [combinación de correspondencia](https://docs.devexpress.com/eXpressAppFramework/400006/document-management/office-module/mail-merge). Permite personalizar la configuración de Combinar correspondencia.

----------

## Módulo de gráfico dinámico

### AnalysisColumnChooserController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.PivotChart.Win.v23.1.dll._  Acciones: ninguna.

El controlador se hereda del controlador  **ColumnChooserControllerBase**  del  [módulo del sistema](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module). Controla el formulario  **Lista de campos**.

----------

### AnalysisDataBindController
Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.PivotChart.v23.1.dll._  Acciones: BindAnalysisData,  **UnbindAnalysisData**.

El controlador se hereda del controlador  [AnalysisViewControllerBase](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#analysisviewcontrollerbase). La acción  **BindAnalysisData**  recupera los objetos que va a analizar la cuadrícula dinámica.  **UnbindAnalysisData**  borra el origen de datos de la cuadrícula dinámica. Use estas acciones para actualizar el origen de datos de la cuadrícula dinámica después de cambiar la propiedad de un objeto Analysis (por ejemplo, la propiedad  **Criteria**  u  **ObjectType**).

----------

### AnalysisPrintPreviewController
Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.PivotChart.Win.v23.1.dll._  Acciones:  **PrintChart**,  **PrintPreviewPivotGrid**.

Hereda el controlador  [WinAnalysisViewControllerBase](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#winanalysisviewcontrollerbase). Las acciones de este controlador se pueden utilizar para imprimir el contenido del editor de propiedades  **AnalysisEditorWin**.

----------

### AnalysisReadOnlyController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.PivotChart.v23.1.dll._  Acciones: ninguna.

Activado para vistas detalladas cuyo tipo de objeto implementa la interfaz  **IAnalysisInfo**. Hace que el control del Editor de análisis sea de sólo lectura, cuando la vista actual es de sólo lectura.

----------

### AnalysisViewControllerBase

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.PivotChart.v23.1.dll._  Acciones: ninguna.

Activado para vistas detalladas cuyo tipo de objeto implementa la interfaz  **IAnalysisInfo**. Si hay un Editor de propiedades de análisis en la vista de detalles actual, actualiza el estado de Acciones.  [AnalysisDataBindController](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#analysisdatabindcontroller),  [WinAnalysisViewControllerBase](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#winanalysisviewcontrollerbase)  y  [PrintChartAnalysisController](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#printchartanalysiscontroller)  heredan este control.

----------

### CustomizeNavigationItemsController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.PivotChart.v23.1.dll._  Acciones: ninguna.

Activado en la ventana principal. Agrega los grupos de navegación Análisis contextual. Estos grupos exponen elementos de navegación que proporcionan a los usuarios finales acceso instantáneo a objetos de análisis relacionados con un determinado elemento de navegación cuando la navegación TreeList está habilitada.

----------

### PivotChartDesignViewController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.PivotChart.Win.v23.1.dll._  Acciones:  **ChartWizard**.

El controlador se hereda del controlador  [AnalysisViewControllerBase](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#analysisviewcontrollerbase). Contiene la acción  **ChartWizard**, que permite a un usuario final ejecutar el Diseñador de gráficos. Esta acción no se activa cuando la vista de detalles actual no contiene un editor de propiedades de análisis. También se deshabilita cuando el control de gráfico del Editor de propiedades de análisis es invisible.

----------

### PrintChartAnalysisController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.PivotChart.Web.v23.1.dll._  Acciones:  **PrintChart**.

El controlador se hereda del controlador  [AnalysisViewControllerBase](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#analysisviewcontrollerbase). Contiene la acción  **PrintChart**, que permite a los usuarios finales imprimir el gráfico contenido en el Editor de propiedades de análisis de la vista de detalles actual. Esta acción se habilita cuando se incluye un Editor de propiedades de análisis en la vista de detalles actual.

----------

### WinAnalysisViewControllerBase

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.PivotChart.Win.v23.1.dll._  Acciones: ninguna.

Un descendiente de  [AnalysisViewControllerBase](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#analysisviewcontrollerbase)  específico de la plataforma. Actualiza el estado Actions cuando cambia la visibilidad del gráfico en el editor de propiedades  **AnalysisEditorWin**. Heredado por  [AnalysisPrintPreviewController](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#analysisprintpreviewcontroller)  y  [PivotChartDesignViewController](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#pivotchartdesignviewcontroller).

----------

## Módulo de cuadrícula dinámica

### ShowChartController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.PivotGrid.Web.v23.1.dll._  Acciones: ninguna.

Activado en vistas de lista visualizadas por el editor de listas  **ASPxPivotGridListEditor**.  **ShowChartAction**  alterna la visibilidad de un gráfico que se muestra debajo del valor dinámico.

----------

## Módulo Reports V2

### CopyPredefinedReportsController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ReportsV2.v23.1.dll._  Acciones:  **CopyPredefinedReport**.

Activado en vistas de lista que muestran objetos  [IReportDataV2](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.IReportDataV2). Proporciona la acción  **CopyPredefinedReport**  que se usa para copiar un informe de sólo lectura predefinido seleccionado en un informe editable.

----------

### CustomizeNavigationItemsController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ReportsV2.v23.1.dll._  Acciones: ninguna.

Activado en la ventana principal. Agrega los grupos de navegación contextuales Informes. Estos grupos exponen elementos de navegación que proporcionan a los usuarios finales acceso instantáneo a los informes relacionados con un determinado elemento de navegación cuando la navegación TreeList está habilitada.

----------

### DeleteReportController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ReportsV2.v23.1.dll._  Acciones: ninguna.

Activado en vistas de lista que muestran objetos  [IReportDataV2](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.IReportDataV2). Deshabilita la acción  **Eliminar**  en  [DeleteObjectsViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.DeleteObjectsViewController)  cuando los objetos seleccionados contienen un informe predefinido.

----------

### NewReportWizardController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ReportsV2.v23.1.dll._  Acciones: ninguna.

Activado en vistas de lista que muestran objetos  [IReportDataV2](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.IReportDataV2). Proporciona a los usuarios finales la capacidad de crear nuevos informes mediante un asistente para informes.

----------

### PreviewReportDialogController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ReportsV2.v23.1.dll._  Acciones:  **Aceptar**,  **Cancelar**.

Un descendiente de  [DialogController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.DialogController)  activado para el cuadro de diálogo de parámetros de informe. Presenta acciones personalizadas  **de Aceptar**  y  **Cancelar**.

**Vea también:**[PreviewReportDialogController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.PreviewReportDialogController)

----------

### PrintSelectionBaseController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ReportsV2.v23.1.dll._  Acciones:  **ShowInReport**.

Activado en todas las vistas. Proporciona el  **ShowInReport**  utilizado para ejecutar  [informes en contexto](https://docs.devexpress.com/eXpressAppFramework/113602/shape-export-print-data/reports/in-place-reports).

**Vea también:**[PrintSelectionBaseController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.PrintSelectionBaseController)  |  [PrintSelectionBaseController.ShowInReportAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.PrintSelectionBaseController.ShowInReportAction)

----------

### ReportsControllerCore

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ReportsV2.v23.1.dll._  Acciones:  **ExecuteReport**.

Activado en vistas de lista que muestran objetos  [IReportDataV2](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.IReportDataV2). Proporciona la acción  **ExecuteReport**  utilizada para ejecutar un informe seleccionado.

**Vea también:**[ReportServiceController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.ReportServiceController)  |  [ReportsControllerCore.ExecuteReportAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.ReportsControllerCore.ExecuteReportAction)

----------

### ReportServiceController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ReportsV2.v23.1.dll._  Acciones: ninguna.

Contiene el código común utilizado para mostrar la ventana Vista previa del informe. Expone el método  [ReportServiceController.ShowPreview](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.ReportServiceController.ShowPreview.overloads)  que invoca la ventana Informe de vista previa.

**Vea también:**[ReportServiceController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.ReportServiceController)

----------

### WebReportsController

Plataforma: ASP.NET Ensamblado de formularios Web Forms:  _DevExpress.ExpressApp.ReportsV2.v23.1.dll._  Acciones:  **PrintReportTo_Mht**,  **PrintReportTo_Rtf**,  **PrintReportTo_Pdf**,  **PrintReportTo_Xls**,  **ShowReportInPreviewWindow**.

Hereda el controlador  [ReportsControllerCore](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#reportscontrollercore). Activado en vistas de lista que muestran objetos  [IReportDataV2](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.IReportDataV2). Proporciona la acción  **ShowReportInPreviewWindow**  y un conjunto de acciones utilizadas para exportar informes a varios formatos. Tenga en cuenta que estas acciones específicas del formato están inactivas a menos que establezca la propiedad  [ReportsAspNetModuleV2.ShowFormatSpecificExportActions](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.Web.ReportsAspNetModuleV2.ShowFormatSpecificExportActions)  en  **true**.

**Véase también:**[WebReportsController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.Web.WebReportsController)

----------

### WebReportServiceController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ReportsV2.Web.v23.1.dll._  Acciones: ninguna.

El controlador se hereda de  [ReportServiceController](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#reportservicecontroller). Contiene un código específico de la plataforma para mostrar la ventana Informe de vista previa en una aplicación ASP.NET de formularios Web Forms.

**Vea también:**  [WebReportServiceController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.Web.WebReportServiceController)

----------

### WinReportsController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.ReportsV2.Win.v23.1.dll._  Acciones: ninguna.

Hereda el controlador  [ReportsControllerCore](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#reportscontrollercore). Activado en vistas de lista que muestran objetos  [IReportDataV2](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.IReportDataV2). Inicializa la acción  [ReportsControllerCore.ExecuteReportAction.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.ReportsControllerCore.ExecuteReportAction)

**Vea también:**  [WinReportsController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.Win.WinReportsController)

----------

### WinReportServiceController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.ReportsV2.Win.v23.1.dll._  Acciones: ninguna.

El controlador se hereda de  [ReportServiceController](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#reportservicecontroller). Contiene el código específico de formularios Windows Forms, que proporciona a los usuarios finales la capacidad de crear, diseñar y ver informes.

**Vea también:**  [WinReportServiceController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ReportsV2.Win.WinReportServiceController)

----------

## Módulo Programador

### SchedulerActionsController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Scheduler.Web.v23.1.dll._  Acciones:  **EditSeries**,  **OpenSeries**  y  **RestoreOccurrence**.

Activado para vistas de lista que muestran los objetos  **IRecurrentEvent**. La acción  **EditSeries**  permite a los usuarios finales modificar toda la serie generada a partir de un  [evento recurrente](https://docs.devexpress.com/eXpressAppFramework/113128/event-planning-and-notifications/scheduler/recurring-events). Utilice la acción  **Editar**  para modificar una instancia de serie determinada. La acción de  **OpenSeries**  permite a los usuarios finales ver el evento recurrente seleccionado actualmente como una serie. Por ejemplo, la colección de recursos cambiada en la vista invocada se cambia para todos los eventos de la serie. La acción  **de edición**  ejecutada para la serie mostrada le permite editar toda la serie. Utilice la acción  **Abrir**  para ver y, a continuación, editar un evento determinado de una serie. La acción  **RestoreOccurrence**  permite a los usuarios finales cambiar el patrón de un evento periódico si se cambió y ya no satisface el patrón de la serie periódica en la que se creó el evento.

----------

### SchedulerChangedOccurrencesController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Scheduler.v23.1.dll._  Acciones: ninguna.

Activado para vistas detalladas que representan objetos  **IEvent**.

Diseñado para crear excepciones a  [los eventos recurrentes](https://docs.devexpress.com/eXpressAppFramework/113128/event-planning-and-notifications/scheduler/recurring-events)  cuando se cambian.

----------

### SchedulerDetailViewActionsController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Scheduler.Web.v23.1.dll._  Acciones:  **EditSeriesDetailView**.

Activado para vistas detalladas que muestran objetos  **IRecurrentEvent**. La acción  **EditSeriesDetailView**  permite a los usuarios finales modificar toda la serie de la ocurrencia de  [eventos recurrentes](https://docs.devexpress.com/eXpressAppFramework/113128/event-planning-and-notifications/scheduler/recurring-events)  que se está viendo actualmente.

----------

### SchedulerModificationsController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Scheduler.Win.v23.1.dll._  Acciones: ninguna.

Activado para vistas detalladas que representan un objeto  **IEvent**. Para uso interno. Cuando se invoca una vista detallada desde la vista de lista mostrada por SchedulerListEditor, se actualiza el editor del programador cuando se cambia o confirma un objeto en la vista de detalles.

----------

### SchedulerListViewControllerBase

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Scheduler.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista que representan objetos  **IEvent**. Este controlador es una base para los  **SchedulerListViewControllers**  específicos de la plataforma.

----------

### SchedulerListViewController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Scheduler.Win.v23.1.dll._  Acciones: ninguna.

El controlador se hereda del controlador  [SchedulerListViewControllerBase](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#schedulerlistviewcontrollerbase). Contiene código específico de formularios Windows Forms que configura el control Programador para que funcione en una aplicación XAF.

----------

### SchedulerListViewController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Scheduler.Web.v23.1.dll._  Acciones: ninguna.

El controlador se hereda del controlador  [SchedulerListViewControllerBase](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#schedulerlistviewcontrollerbase). Contiene ASP.NET código específico de formularios Web Forms que configura el control ASPxScheduler para que funcione en una aplicación XAF.

----------

### SchedulerRecurrenceInfoControllerBase

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Scheduler.v23.1.dll._  Acciones: ninguna.

Activado en Vistas detalladas que representan los objetos que implementan la interfaz  **IRecurrentEvent**. Representa una clase base para formularios Windows Forms y ASP.NET controladores  **SchedulerRecurrenceInfo**. Administra el Editor de propiedades que representa la propiedad  **Periodicidad**.

----------

### SchedulerRecurrenceInfoController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Scheduler.Win.v23.1.dll._  Acciones: ninguna.

El controlador se hereda del controlador  [SchedulerRecurrenceInfoControllerBase](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#schedulerrecurrenceinfocontrollerbase). Contiene código específico de formularios Windows Forms para administrar el Editor de propiedades, que representa la propiedad  **Periodicidad**.

----------

### SchedulerRecurrenceInfoController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.Scheduler.Web.v23.1.dll._  Acciones: ninguna.

El controlador se hereda del controlador  [SchedulerRecurrenceInfoControllerBase](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#schedulerrecurrenceinfocontrollerbase). Contiene ASP.NET código específico de formularios Web Forms para administrar el Editor de propiedades, que representa la propiedad  **Recurrence**.

----------

### SchedulerResourceController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Scheduler.v23.1.dll._  Acciones: ninguna.

Activado en Vistas detalladas que representan los objetos que implementan la interfaz  **IRecurrentEvent**.

Haga que la colección de recursos sea de solo lectura cuando la vista detallada actual visualice una ocurrencia. Una colección de recursos solo se puede editar para toda la serie.

----------

### SchedulerResourceDeletingController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Scheduler.Win.v23.1.dll._  Acciones: ninguna.

Activado para vistas detalladas que muestran objetos  **IResource**. Muestra la advertencia "Actualice la vista de detalles del evento para cargar los cambios" cuando se elimina el objeto actual. El mensaje se  [puede localizar](https://docs.devexpress.com/eXpressAppFramework/112595/localization/localization-basics)  en  **Localización**  |  **Mensajes**  |  **Nodo SchedulerResourceDeletingWarning**.

----------

## Grabador de scripts

### ScriptRecorderControllerBase

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ScriptRecorder.v23.1.dll._  Acciones:  **PauseRecording**,  **ResumeRecord**, SaveScript,  **ShowScript**,  **SaveScriptForDialogWindow**.

La clase base independiente de la plataforma para los controladores WinScriptRecorderController y  [WebScriptRecorderController](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#winscriptrecordercontroller).[](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#webscriptrecordercontroller)

----------

### WebScriptRecorderController

Plataforma: ASP.NET formularios web.  _Ensamblado: DevExpress.ExpressApp.ScriptRecorder.Web.v23.1.dll._  Acciones: heredadas.

El descendiente  [de ScriptRecorderControllerBase](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#scriptrecordercontrollerbase), que implementa una funcionalidad de guardado específica de la plataforma. El script que se va a guardar se escribe en la secuencia de salida HTTP cuando se ejecuta la acción SaveScript o  **SaveScriptForDialogWindow**.  El script se puede guardar utilizando las capacidades del navegador.

----------

### WinScriptRecorderController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.ScriptRecorder.Win.v23.1.dll._  Acciones: heredadas.

El descendiente de  [ScriptRecorderControllerBase](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#scriptrecordercontrollerbase), que implementa la funcionalidad de guardado específica de la plataforma. Cuando se ejecuta la acción SaveScript o  **SaveScriptForDialogWindow**, la secuencia de comandos se guarda mediante el cuadro de diálogo estándar  **Guardar archivo**.

----------

## Sistema de seguridad

### ChangePasswordController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Security.v23.1.dll._  Acciones:  **ChangePasswordByUser**.

Activado para la vista  **de detalles Mis detalles**. La acción  **ChangePasswordByUser**  permite a los usuarios finales cambiar su contraseña. La clase ChangePasswordController llama al método  [IAuthenticationStandardUser.SetPassword](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.Security.IAuthenticationStandardUser.SetPassword(System.String))  para cambiar la contraseña y modificar el objeto seleccionado. La acción  **ChangePasswordByUser**  está disponible cuando un usuario selecciona un objeto que representa la entrada  [SecuritySystem.CurrentUser](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SecuritySystem.CurrentUser)  y su propiedad  [ActionBase.Enabled](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Enabled)  se actualiza dinámicamente cuando se cambia el objeto seleccionado.

**Ver también:**  [Contraseñas en el sistema de seguridad](https://docs.devexpress.com/eXpressAppFramework/112649/data-security-and-safety/security-system/authentication/passwords-in-the-security-system)  |  [SecurityModule.TryUpdateLogonParameters](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Security.SecurityModule.TryUpdateLogonParameters(System.String-System.Object))  |  [PermissionPolicyUser.SetPassword](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.BaseImpl.PermissionPolicy.PermissionPolicyUser.SetPassword(System.String))  |  [PermissionPolicyUser.SetPassword](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.BaseImpl.EF.PermissionPolicy.PermissionPolicyUser.SetPassword(System.String))

----------

### HasRightsToModifyMemberController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Security.v23.1.dll._  Acciones: ninguna.

Activado para vistas detalladas. Hace que los editores de propiedades sean de sólo lectura o editables, en función de los derechos del usuario actual para editar el objeto actual. Cuando un Editor de propiedades representa una propiedad de un objeto establecido en una propiedad de referencia, este Controller lo establece como de sólo lectura o editable, dependiendo de los derechos para editar el objeto al que se hace referencia.

**Vea también:**  [HasRightsToModifyMemberController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Security.HasRightsToModifyMemberController)

----------

### LastAdminController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Security.v23.1.dll._  Acciones: ninguna.

Activado para todas las vistas. Diseñado para la estrategia Simple Security. Produce una excepción cuando se elimina el último usuario "Administrador" porque debe haber al menos un usuario con derechos administrativos.

----------

### MyDetailsController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Security.v23.1.dll._  Acciones:  **MyDetails**.

Activado en la ventana principal. Agrega el elemento  **Mis detalles**  al control de navegación. Cuando un usuario final hace clic en este elemento, se muestra una vista detallada con los detalles del usuario actual. Además, la acción  **MyDetails**  se muestra mediante el  [contenedor de acciones](https://docs.devexpress.com/eXpressAppFramework/112610/ui-construction/action-containers)  "Seguridad", que se agrega a la plantilla  _Default.aspx_  en la versión de la aplicación ASP.NET formularios Web Forms. Esta acción muestra una vista detallada con los detalles del usuario actual.

**Vea también:**  [MyDetailsController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Security.MyDetailsController)

----------

### PermissionsController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Security.v23.1.dll._  Acciones: ninguna.

Activado para vistas cuyo tipo de objeto implementa la interfaz  **IPersistentPermission**. Gestiona la creación de nuevos permisos. Reemplaza los tipos predefinidos de la  **nueva**  acción por los tipos encontrados heredados de la clase  **PermissionBase**. La vista Detalle invocada al utilizar la  **nueva**  acción muestra un nuevo objeto  **PermissionBase**, cuya propiedad  **Permission**  hace referencia al permiso seleccionado en la colección  **Items**  de la  **nueva**  acción.

----------

### RefreshSecurityController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Security.v23.1.dll._  Acciones: ninguna.

Activado para las vistas de lista raíz y de detalle que muestran objetos de usuario. Actualiza el conjunto de permisos cuando se cambia el objeto de usuario actual.

----------

### ResetPasswordController
Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Security.v23.1.dll._  Acciones:  **ResetPasswordAction**.

Activado para vistas raíz. Diseñado para un sistema de seguridad que utiliza la estrategia de autenticación que implementa la interfaz IAuthenticationStandard y el tipo  **de usuario**  que implementa la interfaz  [IAuthenticationStandardUser](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.Security.IAuthenticationStandardUser).  Contiene la acción  **ResetPasswordAction**. Esta acción permite a los usuarios finales que poseen acceso de "escritura" al tipo de usuario establecer una nueva contraseña para los usuarios. Internamente, la acción llama al método IAuthenticationStandardUser.SetPassword y modifica la propiedad  [IAuthenticationStandardUser.ChangePasswordOnFirstLogon.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.Security.IAuthenticationStandardUser.ChangePasswordOnFirstLogon)  [](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.Security.IAuthenticationStandardUser.SetPassword(System.String))Cuando se cambia el objeto de usuario seleccionado, el controlador ResetPasswordController comprueba que el usuario actual tiene permiso para modificar las propiedades  **StoredPassword**  y  **ChangePasswordOnFirstLogon**  del objeto seleccionado y actualiza la propiedad  [ActionBase.Enabled](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Enabled)  de la acción  **ResetPassword**.  Los nombres de estas propiedades no se obtienen dinámicamente. En su lugar, estas cadenas se especifican mediante las propiedades  **PasswordFieldName**  y  **ChangePasswordOnFirstLogonFieldName**, que se declaran estáticas.

**Ver también:**  [Contraseñas en el sistema de seguridad](https://docs.devexpress.com/eXpressAppFramework/112649/data-security-and-safety/security-system/authentication/passwords-in-the-security-system)

----------

## Módulo de State Machine

### DisableStatePropertyController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.StateMachine.v23.1.dll._  Acciones: ninguna.

Activado para vistas detalladas. Prohíbe la edición de la propiedad state estableciendo la propiedad  **AllowEdit**  del editor de propiedades en  **false**.

**Vea también:**  [PropertyEditor.AllowEdit](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Editors.PropertyEditor.AllowEdit)

----------

### StateMachineAppearanceController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.StateMachine.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista y vistas de detalle. Recopila reglas de apariencia condicional declaradas en máquinas de estado asociadas con objetos mostrados actualmente y agrega estas reglas a la lista de reglas de apariencia condicional actualmente activas.

**Ver también:**  [Módulo de máquina de estado](https://docs.devexpress.com/eXpressAppFramework/113336/business-process-management/state-machine-module)  |  [Descripción general del módulo de apariencia condicional](https://docs.devexpress.com/eXpressAppFramework/113286/conditional-appearance)

----------

### StateMachineController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.StateMachine.v23.1.dll._  Acciones:  **ChangeState**.

Activado para vistas de lista y vistas de detalle. Muestra la  [acción](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  **ChangeState**  para los objetos que tienen equipos de estado asociados. La acción permite a los usuarios finales cambiar los estados de los objetos a través de una interfaz de usuario.

**Ver también:**  [Módulo de máquina de estado](https://docs.devexpress.com/eXpressAppFramework/113336/business-process-management/state-machine-module)

----------

### StateMachineControllerBase<T>

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.StateMachine.v23.1.dll._  Acciones: ninguna.

Una clase base abstracta genérica de la que se derivan los controladores del módulo Equipo de estado. Contiene funcionalidad básica, como recuperar máquinas de estado asociadas al objeto que muestra la  [vista](https://docs.devexpress.com/eXpressAppFramework/112611/ui-construction/views)  actual.

**Vea también:**  [View.CurrentObject](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.View.CurrentObject)

----------

## Módulo de editores de listas de árboles

### CategoryController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.TreeListEditors.Win.v23.1.dll._  Acciones: ninguna.

Activado para todas las vistas. Funciona con clases empresariales que implementan la interfaz  [ICategorizedItem](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.General.ICategorizedItem). Este controlador establece automáticamente una categoría para los objetos recién creados.

----------

### TreeListEditorColumnChooserController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.TreeListEditors.Win.v23.1.dll._  Acciones: ninguna.

El controlador se hereda del controlador  **ColumnChooserControllerBase**  del  [módulo del sistema](https://docs.devexpress.com/eXpressAppFramework/113141/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-the-system-module). Configura el formulario de personalización del Editor de lista de árboles y admite su funcionalidad. Este formulario se puede invocar seleccionando un selector de columnas en el menú contextual de la cuadrícula.

----------

### TreeListEditorRootValueController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.TreeListEditors.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista anidadas. Permite que la colección secundaria de un objeto  [ITreeNode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.General.ITreeNode)  se muestre mediante el control TreeList en la vista Detalle del objeto.

----------

### TreeNodeController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.TreeListEditors.Win.v23.1.dll._  Acciones: ninguna.

Activado para vistas raíz. Funciona con clases de negocio que implementan la interfaz  [ITreeNode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.General.ITreeNode). Este controlador vincula un objeto recién creado a su padre.

----------

## Módulo de validación

### ActionValidationController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Validation.v23.1.dll._  Acciones: ninguna.

Extiende el nodo IModelAction del modelo de aplicación con la propiedad  [IModelActionValidationContexts.ValidationContexts.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Validation.IModelActionValidationContexts.ValidationContexts)  [](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelAction)Se suscribe al evento  **Ejecución**  de todas las acciones disponibles en el  [marco](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Frame)  actual. En el controlador de eventos, comprueba las reglas de validación asociadas al contexto establecido para la propiedad  **ValidationContexts de**  la acción. Si se infringe al menos una regla, cancela la ejecución de la acción y genera una excepción de validación.

----------

### CheckContextsAvailableController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Validation.v23.1.dll._  Acciones: ninguna.

Activado en todas las ventanas. Desactiva la acción  [ShowAllContextsController's de ShowAllContexts](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#showallcontextscontroller), si no se especifican reglas de validación para el tipo de objeto  **de**  la vista actual.

----------

### CustomizeErrorMessageColumnController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Validation.Win.v23.1.dll._  Acciones: ninguna.

Este controlador utiliza las propiedades  [GridListEditor.GridView](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.Editors.GridListEditor.GridView)  y  **XafGridView.ErrorMessages**  para mostrar el icono Error/Advertencia/Información en la columna  **Descripción**  de la vista de lista  **RuleSetValidationResultItem_ByTarget_ListView**  (vea  [Declarar reglas de validación](https://docs.devexpress.com/eXpressAppFramework/113251/validation/declare-validation-rules)). Este controlador no está diseñado para funcionar con otros editores de listas. Desactívelo si necesita utilizar otra clase de Editor de listas para mostrar objetos en la vista de lista 'RuleSetValidationResultItem_ByTarget_ListView'.

----------

### ContextValidationResultController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Validation.Win.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista que muestran objetos  **ContextValidationResult**. Personaliza la configuración de  [ListView.Editor](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ListView.Editor), de modo que el alto de cada fila se ajuste automáticamente para mostrar el contenido de sus celdas.

----------

### PersistenceValidationController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Validation.v23.1.dll._  Acciones: ninguna.

Se suscribe a los eventos BaseObjectSpace.ObjectDeleting y  [BaseObjectSpace.Committing.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.BaseObjectSpace.Committing)  [](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.BaseObjectSpace.ObjectDeleting)En los controladores de eventos, comprueba las reglas de validación asociadas con los contextos de validación DefaultContexts.Save y  [DefaultContexts.Delete.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Validation.DefaultContexts)  [](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Validation.DefaultContexts)Si se infringe al menos una regla, cancela la ejecución de la operación Guardar/Eliminar y genera una excepción de validación.

**Vea también:**  [PersistenceValidationController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Validation.PersistenceValidationController)

----------

### PreventValidationDetailsAccessController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Validation.v23.1.dll._  Acciones: ninguna.

Activado para vistas de lista que muestran los objetos  **DisplayableValidationResultItem**. Impide invocar la vista detallada  **DisplayableValidationResultItem**  mediante la acción ListViewShowObject (vea  [ListViewProcessCurrentObjectController.ProcessCurrentObjectAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ListViewProcessCurrentObjectController.ProcessCurrentObjectAction)) cuando la propiedad  **ValidationModule.AllowValidationAccess**  se establece en  **false**.

----------

### ResultsHighlightController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Validation.v23.1.dll._  Acciones: ninguna.

Activado para todas las vistas. Cuando se rompen las reglas de validación, este controlador agrega mensajes a la colección  [View.ErrorMessages.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.View.ErrorMessages)  Esto da como resultado la visualización de imágenes de error (![Error](https://docs.devexpress.com/eXpressAppFramework/images/error115925.png)) cerca de los editores de propiedades con datos no válidos.

----------

### SuppressToolBar

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Validation.Win.v23.1.dll._  Acciones: ninguna.

Se activa para las vistas de lista anidadas de las vistas de detalle que se muestran cuando se rompe una regla de validación o se ejecuta la acción  **ShowAllContexts**. Oculta la barra de herramientas de la vista de lista mediante los métodos de  **ToolbarVisibilityController**.

----------

### ShowAllContextsController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Validation.v23.1.dll._  Acciones:  **ShowAllContexts**.

Activado para todas las vistas. La acción  **ShowAllContexts**  comprueba todas las reglas diseñadas para el tipo de objeto de la vista actual. En una vista de lista, los objetos seleccionados actualmente están sujetos a comprobación; en una vista detallada, se comprueba el objeto actual. El resultado de la comprobación de las reglas se muestra en una ventana invocada, donde todas las reglas marcadas se agrupan por contexto.

----------

### ShowAllContextsController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Validation.v23.1.dll._  Acciones: ninguna.

Activado en todas las ventanas. Representa el descendiente  **de DiagnosticInfoProviderBase**. Proporciona la información sobre las reglas que están disponibles actualmente en el modelo de aplicación para  **DiagnosticInfoController**.  **DiagnosticInfoController**  agrega el elemento de acción de opción única Información  **de reglas**  **Acción DiagnosticInfo**. Cuando un usuario final selecciona este elemento, la información recopilada se muestra en una ventana invocada.

**Vea también:**  [Determinar por qué una acción, controlador o editor está inactivo](https://docs.devexpress.com/eXpressAppFramework/112818/ui-construction/controllers-and-actions/determine-why-an-action-controller-or-editor-is-inactive)

----------

### ValidationResultsShowingController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Validation.Win.v23.1.dll._  Acciones: ninguna.

Activado en la ventana principal. Cuando se produce una excepción de validación, muestra un error de validación especial Vista detallada en la ventana de error de excepción.

----------

## Ver módulo de variantes

### ChangeVariantController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ViewVariantsModule.v23.1.dll._  Acciones:  **ChangeVariant**.

Activado para todas las vistas. La acción  **ChangeVariant**  permite a los usuarios finales cambiar entre variantes de vista predefinidas de un tipo de objeto de negocio determinado. Sus elementos son especificados por el nodo  [IModelVariants](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewVariantsModule.IModelVariants)  del modelo de aplicación.

**Vea también:**  [ChangeVariantController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewVariantsModule.ChangeVariantController)

----------

### CustomizeNavigationItemsController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.ViewVariantsModule.v23.1.dll._  Acciones: ninguna.

Activado en la ventana principal. Agrega los grupos de navegación contextuales Variantes de vista. Estos grupos exponen elementos de navegación, proporcionando a los usuarios finales acceso instantáneo a Ver variantes relacionadas con un determinado elemento de navegación cuando la navegación TreeList está habilitada. Amplía el modelo de aplicación con la interfaz  [IModelNavigationItemsVariantSettings](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewVariantsModule.IModelNavigationItemsVariantSettings).

----------

## Módulo de flujo de trabajo

### ActivationController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Workflow.v23.1.dll._  Acciones:  **Activar**,  **Desactivar**.

Activado en todas las vistas que muestran objetos  **ISupportIsActive**. Agrega las acciones  **Activate**  y  **Deactivate**  que establecen la propiedad  **ISupportIsActive.IsActive**  en  **true**  y  **false**, respectivamente.

----------

### DebugWorkflowController

Plataforma: Windows Forms.  _Ensamblado: DevExpress.ExpressApp.Workflow.Win.v23.1.dll._  Acciones:  **DebugWorkflow**,  **ToggleBreakpoint**,  **StopDebug**.

Activado en las vistas de detalle  **de IWorkflowDefinition**. Agrega  [acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  de depuración de flujo de trabajo en vistas detalladas de definición de flujo de trabajo en aplicaciones de Windows Forms. Estas acciones se activan después de inicializar la actividad de un flujo de trabajo abriendo la pestaña Diseñador. Las acciones se pueden usar para probar flujos de trabajo y diagnosticar excepciones.

**Consulte también:**  [Módulo de flujo de trabajo](https://docs.devexpress.com/eXpressAppFramework/113343/business-process-management/workflow-module)

----------

### DisableRunningWorkflowInstanceInfoCreateByUserController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Workflow.v23.1.dll._  Acciones: ninguna.

Activado en todas las ventanas. Se suscribe a los eventos  [NewObjectViewController.CollectCreatableItemTypes y NewObjectViewController.CollectDescendantTypes](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController.CollectCreatableItemTypes)  de  [NewObjectViewController.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController.CollectDescendantTypes)  [](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController)Quita todos los objetos  **IRunningWorkflowInstanceInfo**  de la lista de tipos creables.

----------

### RunningInstanceController
Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Workflow.v23.1.dll._  Acciones: CancelWorkflowInstance,  **TerminateWorkflowInstance**, SuspendWorkflowInstance,  **ResumeWorkflowInstance**.

Activado en vistas de lista y vistas de detalle que muestran objetos  **IRunningWorkflowInstanceInfo**. Agregar acciones que pueden usar los administradores de aplicaciones para administrar instancias de flujo de trabajo en ejecución.

**Consulte también:**  [Módulo de flujo de trabajo](https://docs.devexpress.com/eXpressAppFramework/113343/business-process-management/workflow-module)

----------

### RunningWorkflowInstanceInfoControllerBase<T>

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Workflow.v23.1.dll._  Acciones: ninguna.

Una clase base abstracta que contiene funcionalidad básica para Controllers que proporciona información sobre la ejecución de instancias de flujo de trabajo. Se activa en las vistas de objeto especificadas por el parámetro de tipo genérico, que muestra objetos  **IDCRunningWorkflowInstanceInfo**. Prohíbe a los usuarios finales editar objetos.

----------

### RunningWorkflowInstanceInfoViewer

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Workflow.v23.1.dll._  Acciones:  **ShowWorkflowInstances**.

Activado en vistas de lista y vistas de detalle. Agrega la acción  **ShowWorkflowInstances**  que muestra todas las instancias de flujo de trabajo en ejecución para el objeto actual.

----------

### StartWorkflowController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Workflow.v23.1.dll._  Acciones: ninguna.

Activado en la ventana principal (consulte  [Window.IsMain](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Window.IsMain)). Realiza un seguimiento de la creación de  [espacios de objetos](https://docs.devexpress.com/eXpressAppFramework/113707/data-manipulation-and-business-logic/object-space)  controlando el evento  [XafApplication.ObjectSpaceCreated.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.XafApplication.ObjectSpaceCreated)  Crea una solicitud de ejecución de instancia de flujo de trabajo cuando corresponde.

----------

### TrackingVisualizationController

Plataforma: independiente de la plataforma.  _Ensamblado: DevExpress.ExpressApp.Workflow.v23.1.dll._  Acciones: ninguna.

Un descendiente  [de RunningWorkflowInstanceInfoControllerBase<T>](https://docs.devexpress.com/eXpressAppFramework/113142/ui-construction/controllers-and-actions/built-in-controllers-and-actions/built-in-controllers-and-actions-in-extra-modules#runningworkflowinstanceinfocontrollerbaset). Activado en la ejecución de vistas detalladas de información de instancia de flujo de trabajo. Resalta una actividad seleccionada en la vista de lista anidada Registros de seguimiento en el  [WorkflowVisualizationPropertyEditor](https://docs.devexpress.com/eXpressAppFramework/113576/business-model-design-orm/data-types-supported-by-built-in-editors/miscellaneous-property-types)  asociado.



# Personalizar los parámetros de visualización de Mostrarpara acciones simples integradas

Para personalizar  [ShowViewParameters](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ShowViewParameters), controle el evento  [Executed](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Executed). Si un controlador propietario de acciones tiene un evento que le permite personalizar  [ShowViewParameters](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ShowViewParameters), use este evento en lugar del evento. `Executed`

En el ejemplo siguiente se muestra cómo mostrar un control DetailView en una ventana modal (emergente) para las acciones  **Nuevo**  y  **Editar**  en la aplicación XAF de Windows Forms.

```csharp
using DevExpress.ExpressApp;
using DevExpress.ExpressApp.SystemModule;

namespace MainDemo.Module.Controllers {
    public class ChangeWindowTypeToModalController : ViewController<ListView> {
        private void NewObjectAction_Executed(object sender, DevExpress.ExpressApp.Actions.ActionBaseEventArgs e) {
            e.ShowViewParameters.TargetWindow = TargetWindow.NewModalWindow;
        }
        private void ListViewProcessCurrentObjectController_CustomizeShowViewParameters(object sender, CustomizeShowViewParametersEventArgs e) {
            e.ShowViewParameters.TargetWindow = TargetWindow.NewModalWindow;
        }
        protected override void OnActivated() {
            base.OnActivated();
            NewObjectViewController newObjectViewController = Frame.GetController<NewObjectViewController>();
            if (newObjectViewController != null) {
                newObjectViewController.NewObjectAction.Executed += NewObjectAction_Executed;
            }
            ListViewProcessCurrentObjectController listViewProcessCurrentObjectController = Frame.GetController<ListViewProcessCurrentObjectController>();
            if(listViewProcessCurrentObjectController != null) {
                listViewProcessCurrentObjectController.CustomizeShowViewParameters += ListViewProcessCurrentObjectController_CustomizeShowViewParameters;
            }
        }
        protected override void OnDeactivated() {
            NewObjectViewController newObjectViewController = Frame.GetController<NewObjectViewController>();
            if(newObjectViewController != null) {
                newObjectViewController.NewObjectAction.Executed -= NewObjectAction_Executed;
            }
            ListViewProcessCurrentObjectController listViewProcessCurrentObjectController = Frame.GetController<ListViewProcessCurrentObjectController>();
            if(listViewProcessCurrentObjectController != null) {
                listViewProcessCurrentObjectController.CustomizeShowViewParameters -= ListViewProcessCurrentObjectController_CustomizeShowViewParameters;
            }
            base.OnDeactivated();
        }
    }
} 

```


# Controladores (UI Logic & Interaction)


El proceso de creación de aplicaciones  **de eXpressApp Framework**  se puede dividir en varios pasos. El primer paso: implementación del modelo de negocio, el segundo, personalización predeterminada de la interfaz de usuario, y el tercero es el desarrollo de características personalizadas (para cambiar el flujo de la aplicación e implementar la interacción personalizada del usuario final). Para el último paso, deberá usar Controllers.  **eXpressApp Framework**  proporciona una serie de controladores integrados que se utilizan al generar la interfaz de usuario predeterminada. Por ejemplo, las funciones de validación, navegación y búsqueda ya están incluidas en la interfaz de usuario predeterminada. Para implementar una característica personalizada, deberá crear un Controller, una clase derivada de la clase Controller,  [ViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController)  o  [WindowController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Controller).[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.WindowController)  En este tema se explica cómo hacerlo correctamente.

NOTA

En las aplicaciones .**NET 6**, el diseñador  **de Controller** no está disponible debido a los cambios en la arquitectura del diseñador de Visual Studio. Puede realizar tareas relacionadas en el código:

-   [Agregar un controlador en el código](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers#add-a-controller-in-code) | [Agregar una acción a un controlador](https://docs.devexpress.com/eXpressAppFramework/400495/ui-construction/controllers-and-actions/actions/perform-common-tasks-with-xaf-actions#add-an-action-to-a-controller)
-   [Agregar acciones y controladores con unas pocas pulsaciones de teclas mediante plantillas de CodeRush](https://docs.devexpress.com/CodeRushForRoslyn/403133/coding-assistance/code-templates/xaf-templates?v=22.2)

## Descripción general de los controladores

Los controladores tienen dos propósitos principales:

-   **Realice acciones específicas cuando se crea o destruye una ventana (marco).**
    
    Cuando se crea una  [ventana (Frame),](https://docs.devexpress.com/eXpressAppFramework/112608/ui-construction/windows-and-frames)  se activan todos los Controllers destinados a ella, lo que significa que se generan sus eventos especiales (consulte  [Controller.Activated](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Controller.Activated)). Puede controlar estos eventos para implementar características relacionadas con la ventana actual (marco) o su  [vista](https://docs.devexpress.com/eXpressAppFramework/112611/ui-construction/views). Cuando se elimina una ventana (marco), sus controladores se desactivan, lo que significa que se genera su evento especial (consulte  [Controlador.Desactivado](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Controller.Deactivated)). Esto le permite realizar acciones específicas al cerrar una ventana o deshacerse de un marco.
    
-   **Amplíe la interfaz de usuario**.
    
    En la mayoría de los casos, las características exigen la interacción del usuario final. Para este propósito, los Controladores pueden servir como contenedores para  [Acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions). Las acciones son objetos que representan elementos abstractos de la interfaz de usuario y se pueden mostrar en una interfaz de usuario mediante controles reales: un botón, cuadro combinado, submenú, etc. Para responder a las manipulaciones de un usuario final con un control de acción, controle los eventos de la acción correspondiente.
    

Al igual que con la mayoría de las entidades XAF, la información sobre los controladores que se encuentra en los  [módulos](https://docs.devexpress.com/eXpressAppFramework/118046/application-shell-and-base-infrastructure/application-solution-components/modules)  de la aplicación se carga en el  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works). Puede acceder a la configuración del controlador en el nodo  [IModelControllers](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelControllers). Consulte la descripción de este nodo para obtener información sobre posibles personalizaciones.

## Controlador de ventana, controlador de vista y clases decontrolador

Físicamente, los controladores son descendientes de la clase  [Controller](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Controller). Tenga en cuenta que no necesitará crear descendientes directos de esta clase para proporcionar controladores para la aplicación. En su lugar, tratará con dos descendientes predefinidos: ViewController (incluidas sus versiones genéricas: ViewController<ViewType> y ObjectViewController[<ViewType, ObjectType>](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ObjectViewController-2)) y  [WindowController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController).  [](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController-1)[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.WindowController)Estas clases proporcionan los eventos  [Controller.Activated](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Controller.Activated)  y  [Controller.Deactivated](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Controller.Deactivated)  que permiten realizar acciones específicas cuando se crea o destruye la ventana (marco) correspondiente. Para tener acceso a Window (Frame) o su View en estos controladores de eventos, la clase ViewController ofrece las propiedades Controller.Frame y  **ViewController.View**[.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Controller.Frame)  [](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController.View)La clase WindowController expone la ventana correspondiente mediante la propiedad  [WindowController.Window.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.WindowController.Window)

Aunque estas dos clases tienen mucho en común, debes usarlas en diferentes escenarios. Más sobre esto ahora.

### Ver controladores

Los controladores de vista están diseñados para implementar características (funciones de filtro o búsqueda, etc.) para vistas. Básicamente, debe usar un controlador de vista cada vez que necesite implementar una función compatible con datos.

Las acciones contenidas en un controlador de vista acompañan a las vistas para las que el controlador está activado. Por ejemplo, si se activa un Controller para una vista de lista anidada (vista de lista de una propiedad de colección), sus acciones se adjuntarán a esta vista, no a toda la ventana.

![NestedFrameTemplate](https://docs.devexpress.com/eXpressAppFramework/images/nestedframetemplate115362.png)

Los controladores de vista se activan tanto para Windows como para marcos cuando una vista se establece en una ventana o marco. Sin embargo, puede especificar el tipo o ID de la vista que necesita incluir en la ventana o marco. Para ello, utilice las siguientes propiedades de  **ViewController**:

-   [ViewController.TargetViewType](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController.TargetViewType)
    
    Especifica el tipo de vista: Vista de lista, Vista de detalle o cualquier vista. De forma predeterminada, el valor de esta propiedad es  **Any**.
    
-   [ViewController.TargetViewNesting](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController.TargetViewNesting)
    
    Especifica si se permite activar el controlador actual para la vista raíz, la vista anidada o cualquier vista. De forma predeterminada, el valor de esta propiedad es  **Any**.
    
-   [ViewController.TargetObjectType](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController.TargetObjectType)
    
    Especifica el tipo de objeto persistente mostrado por la vista.
    
-   [ViewController.TargetViewId](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController.TargetViewId)
    
    Especifica el identificador de vista. Este ID se especifica en el  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works).
    

### Controladores de ventana

Los controladores de ventana se activan cuando se crea una ventana o un marco, y están destinados a implementar características para Windows, es decir, características que no están relacionadas con vistas específicas. Por lo tanto, estas características no están relacionadas con los datos en la mayoría de los casos. Un ejemplo puede ser un controlador que cambia la apariencia de un control de navegación en la ventana principal (consulte el tema  [Cómo: Acceder al control de navegación](https://docs.devexpress.com/eXpressAppFramework/112617/application-shell-and-base-infrastructure/navigation/how-to-access-navigation-control)  para ver el ejemplo). Las acciones contenidas en los controladores de ventana siempre se muestran en una ventana, independientemente de la vista que se muestre actualmente.

Los controladores de ventana solo están activados para Windows. Además, puede especificar el tipo de ventana para el que se activará el controlador. Para ello, utilice la propiedad  [WindowController.TargetWindowType del Controller.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.WindowController.TargetWindowType)  Su valor se puede establecer en Cualquiera, Principal o Hijo. Tenga en cuenta que las aplicaciones XAF tienen una sola ventana principal, que se muestra primero. El resto son ventanas para niños.

## Implementar controladores personalizados

Si un Controller, junto con sus Acciones, es independiente de la interfaz de usuario, debe implementarse en un  [proyecto de Módulo](https://docs.devexpress.com/eXpressAppFramework/118045/application-shell-and-base-infrastructure/application-solution-components/application-solution-structure). Al mismo tiempo, puede haber una tarea específica de la interfaz de usuario. En este caso, se debe desarrollar un controlador en un módulo específico de la interfaz de usuario. Si la solución no contiene este proyecto, impleméntelo en un  [proyecto de aplicación](https://docs.devexpress.com/eXpressAppFramework/118045/application-shell-and-base-infrastructure/application-solution-components/application-solution-structure).

Los controladores de XAF admiten  [la inserción de dependencias](https://docs.devexpress.com/eXpressAppFramework/404364/application-shell-and-base-infrastructure/dependency-injection-in-xaf-applications)  en aplicaciones .NET 6+ ASP.NET Core Blazor y Windows Forms.

### Agregar un controlador en tiempo de diseño

Puede usar las plantillas de Controller listas para usar para crear un Controller personalizado.

1.  En  **el Explorador de soluciones**, seleccione un proyecto al que desee agregar un Controller. Haga clic con el botón secundario en la carpeta  _Controllers_  ubicada dentro del proyecto para invocar el menú contextual y seleccione  **Agregar elemento DevExpress**  |  **Nuevo artículo...**  para invocar la  **Galería de plantillas de DevExpress**. Se mostrará el siguiente cuadro de diálogo.
    
    ![Tutorial_EF_Lesson1_1](https://docs.devexpress.com/eXpressAppFramework/images/add-devexpress-item-view-controller.png)
    
2.  Seleccione Controlador de  **ventana o Controlador de**  **vista**, especifique su nombre y presione el botón  **Agregar elemento**  para agregar un nuevo controlador al proyecto.
3.  **Para los proyectos de .NET Framework.**  Utilice el  **Diseñador**  del controlador para agregar acciones desde el  **Cuadro de herramientas**  y personalizar las propiedades del controlador mediante la ventana  **Propiedades**. Para invocar el Diseñador, puede hacer clic con el botón secundario en el archivo del controlador en el  **Explorador de soluciones**  y seleccionar el elemento de menú contextual del  **Diseñador**  de  **vistas**.

Tenga en cuenta que el diseñador de Visual Studio no se puede utilizar con controladores genéricos. Consulte la descripción de ViewController<ViewType> u  [ObjectViewController<ViewType, ObjectType>](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ObjectViewController-2)  para obtener información sobre cómo hacer que el diseñador esté disponible.[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController-1)  No olvide reconstruir la solución después de realizar cambios en el Diseñador. De lo contrario, no los verá en el Editor de modelos.

>NOTA
>
>La carpeta Controllers se agrega a los proyectos de módulo o aplicación para su comodidad, para mantener todos sus  _Controllers_ juntos. Mientras tanto, el uso de esta carpeta no es un requisito. La estructura de su proyecto depende totalmente de usted.

**Para los proyectos de .NET Framework.**  Un Controller creado con una plantilla se declara como una clase parcial y contiene un archivo de código subyacente que contendrá las personalizaciones realizadas con el  **Diseñador**.

### Agregar un controlador en el código

1.  Agregue una nueva clase a un proyecto de módulo (MySolution.Module o  _MySolution_._***. Módulo_). Si la solución no contiene proyectos de módulo específicos de la plataforma, puede agregar una clase al proyecto de  [aplicación](https://docs.devexpress.com/eXpressAppFramework/118045/application-shell-and-base-infrastructure/application-solution-components/application-solution-structure). Herede esta clase de una de las siguientes clases:
    
    -   [ViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController)
    -   [ViewController<ViewType>](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController-1)
    -   [ObjectViewController<ViewType, ObjectType>](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ObjectViewController-2)
    -   [WindowController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.WindowController)
2.  En el constructor del Controller, especifique la Vista o Ventana de destino y cree Acciones.
    
3.  Implemente lógica personalizada en los controladores de eventos de Controller.

#### Controlador de vista



```csharp
using DevExpress.ExpressApp;
using DevExpress.Persistent.BaseImpl;
// ...
public class MyViewController : ViewController {
    public MyViewController() : base() {
        TargetObjectType = typeof(Person);
        TargetViewType = ViewType.ListView;
    }
    protected override void OnActivated() {
        base.OnActivated();
        // Perform various tasks depending on the target View.
        View.CustomizeViewItemControl(this, MyMethod);
    }

    private void MyMethod(ViewItem viewItem) {
         // Access and customize the target View control.
    }
    protected override void OnDeactivated() {
        // Unsubscribe from previously subscribed events and release other references and resources.
        base.OnDeactivated();
    }
}

```

#### Controlador de ventana



```csharp
using DevExpress.ExpressApp;
using DevExpress.ExpressApp.SystemModule;
// This Controller changes a caption in a main application window
public class CustomizeWindowController : WindowController {
    public CustomizeWindowController() {
        TargetWindowType = WindowType.Main;
    }
    protected override void OnActivated() {
        base.OnActivated();
        WindowTemplateController controller = Frame.GetController<WindowTemplateController>();
        controller.CustomizeWindowCaption += Controller_CustomizeWindowCaption;
    }
    private void Controller_CustomizeWindowCaption(object sender, CustomizeWindowCaptionEventArgs e) {
        e.WindowCaption.Text = "My Custom Caption";
    }
    protected override void OnDeactivated() {
        base.OnDeactivated();
        WindowTemplateController controller = Frame.GetController<WindowTemplateController>();
        controller.CustomizeWindowCaption -= Controller_CustomizeWindowCaption;
    }
}

```

>NOTA
>
>[CodeRush  ](https://www.devexpress.com/products/coderush/)le permite agregar acciones y controladores con unas pocas pulsaciones de teclas. Para obtener información sobre las plantillas de código para XAF, consulte el siguiente tema de ayuda: [Plantillas XAF](https://docs.devexpress.com/CodeRushForRoslyn/403133/coding-assistance/code-templates/xaf-templates?v=22.2).


# Acciones (comandos de menú)
## Definición de acción

Las acciones son elementos abstractos de  [la interfaz de usuario](https://docs.devexpress.com/eXpressAppFramework/112607/ui-construction/ui-element-overview)  que permiten a los usuarios finales interactuar con aplicaciones XAF.

En la interfaz de usuario, XAF puede mostrar acciones como los siguientes controles:

- Un botón de barra de herramientas
![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/86faf858-3c4a-46a2-8280-54ab48d0eca1)

- Un elemento de menú contextual
![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/eb57041e-445c-49f8-8455-4c4ea55bb732)

-  Un editor 
![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/47da527b-bb1c-4b6d-b26a-9d966abb2faa)

- Un botón simple 
![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/ba6a235d-41c0-46be-8428-651f00fbd3f6)

- Otros controles

Puede acceder y modificar el control asociado a una acción en el controlador de eventos  [ActionBase.CustomizeControl.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.CustomizeControl)

## Tipos de acción

Las acciones XAF pueden ser de diferentes tipos:

-   [SimpleAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.SimpleAction)
-   [PopupWindowShowAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.PopupWindowShowAction)
-   [Acción parametrizada](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ParametrizedAction)
-   [SingleChoiceAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.SingleChoiceAction)
-   [ActionUrl](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionUrl)

También puede  [crear tipos de acción personalizados y controles personalizados](https://docs.devexpress.com/eXpressAppFramework/400495/ui-construction/controllers-and-actions/actions/perform-common-tasks-with-xaf-actions#create-custom-action-types-and-custom-controls).

Las acciones se encuentran en  [contenedores de acciones](https://docs.devexpress.com/eXpressAppFramework/112610/ui-construction/action-containers). Para tener acceso a todas las acciones de un contenedor de acciones, utilice la propiedad  [IActionContainer.Actions](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Templates.IActionContainer.Actions)  del contenedor.

Las acciones pertenecen a  [los Controladores](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers). Utilice la propiedad Controller.Actions para tener acceso a la colección Action de un  [Controller.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Controller.Actions)

## Ubicaciones de acción

XAF coloca Acciones en las ubicaciones que se enumeran a continuación.

-   [Menú: Barra de herramientas principal](https://docs.devexpress.com/eXpressAppFramework/400496/ui-construction/controllers-and-actions/actions/access-actions-in-different-ui-areas/menu-main-toolbar)
-   [Menú: Barra de herramientas anidada](https://docs.devexpress.com/eXpressAppFramework/400497/ui-construction/controllers-and-actions/actions/access-actions-in-different-ui-areas/menu-nested-toolbar)
-   [Navegación](https://docs.devexpress.com/eXpressAppFramework/400499/ui-construction/controllers-and-actions/actions/access-actions-in-different-ui-areas/navigation)
-   [Ventana emergente](https://docs.devexpress.com/eXpressAppFramework/400500/ui-construction/controllers-and-actions/actions/access-actions-in-different-ui-areas/popup-window)
-   [Vista de lista de búsqueda](https://docs.devexpress.com/eXpressAppFramework/400501/ui-construction/controllers-and-actions/actions/access-actions-in-different-ui-areas/lookup-list-view)
-   [Vista detallada](https://docs.devexpress.com/eXpressAppFramework/400502/ui-construction/controllers-and-actions/actions/access-actions-in-different-ui-areas/detail-view-layout-action)
-   [Otros menús](https://docs.devexpress.com/eXpressAppFramework/400498/ui-construction/controllers-and-actions/actions/access-actions-in-different-ui-areas/other-menus)

## Implementación y personalización de acciones

Puede personalizar las acciones de la aplicación de dos maneras:

-   [Realizar tareas comunes con acciones XAF](https://docs.devexpress.com/eXpressAppFramework/400495/ui-construction/controllers-and-actions/actions/perform-common-tasks-with-xaf-actions)
-   [Acciones de acceso en diferentes áreas de la interfaz de usuario](https://docs.devexpress.com/eXpressAppFramework/400494/ui-construction/controllers-and-actions/actions/access-actions-in-different-ui-areas)

También puede  [crear tipos de acción personalizados y controles personalizados](https://docs.devexpress.com/eXpressAppFramework/400495/ui-construction/controllers-and-actions/actions/perform-common-tasks-with-xaf-actions#create-custom-action-types-and-custom-controls).


# Realizar tareas comunes con acciones XAF


En este artículo se describen las tareas más comunes con  [acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  XAF.

-   [Usar la configuración de acciones](https://docs.devexpress.com/eXpressAppFramework/400495/ui-construction/controllers-and-actions/actions/perform-common-tasks-with-xaf-actions#use-action-settings)
-   [Agregar una acción a un controlador](https://docs.devexpress.com/eXpressAppFramework/400495/ui-construction/controllers-and-actions/actions/perform-common-tasks-with-xaf-actions#add-an-action-to-a-controller)
-   [Agregar una acción aplicando el atributo Action a un método de clase empresarial](https://docs.devexpress.com/eXpressAppFramework/400495/ui-construction/controllers-and-actions/actions/perform-common-tasks-with-xaf-actions#add-an-action-by-applying-the-action-attribute-to-a-business-class-method)
-   [Personalizar una acción en el modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/400495/ui-construction/controllers-and-actions/actions/perform-common-tasks-with-xaf-actions#customize-an-action-in-the-application-model)
-   [Personalizar una acción en el código](https://docs.devexpress.com/eXpressAppFramework/400495/ui-construction/controllers-and-actions/actions/perform-common-tasks-with-xaf-actions#customize-an-action-in-code)
-   [Crear tipos de acción personalizados y controles personalizados](https://docs.devexpress.com/eXpressAppFramework/400495/ui-construction/controllers-and-actions/actions/perform-common-tasks-with-xaf-actions#create-custom-action-types-and-custom-controls)
-   [Solucionar problemas de acciones](https://docs.devexpress.com/eXpressAppFramework/400495/ui-construction/controllers-and-actions/actions/perform-common-tasks-with-xaf-actions#troubleshoot-actions)

## Agregar una acción a un controlador

>NOTA
>
>En las aplicaciones .**NET Core**, es imposible agregar una acción a un controlador en el diseñador  **del controlador** debido a los cambios en la arquitectura del diseñador de Visual Studio. Puede agregar una acción en el código como se describe a continuación.

Si necesita una acción que se aplique a varios objetos de negocio y tome los datos proporcionados por el usuario, agregue esta  [acción](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  a un  [Controller](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers).

-   [Agregar una acción parametrizada](https://docs.devexpress.com/eXpressAppFramework/402155/getting-started/in-depth-tutorial-blazor/add-actions-menu-commands/add-a-parametrized-action)
-   [Agregar una acción que muestre una ventana emergente](https://docs.devexpress.com/eXpressAppFramework/402158/getting-started/in-depth-tutorial-blazor/add-actions-menu-commands/add-an-action-that-displays-a-pop-up-window)
-   [Agregar una acción con selección de opción](https://docs.devexpress.com/eXpressAppFramework/402159/getting-started/in-depth-tutorial-blazor/add-actions-menu-commands/add-an-action-with-option-selection)

También puede agregar una acción a un controlador en código.



```csharp
using DevExpress.ExpressApp;
using DevExpress.ExpressApp.Actions;
using DevExpress.Persistent.Base;
// ...
public class CustomViewController : ViewController {
    public CustomViewController() {
        SimpleAction customAction = new SimpleAction(this, "CustomAction", PredefinedCategory.View);
        // or
        customAction.Category = PredefinedCategory.View.ToString();
        // or 
        customAction.Category = "View";
        // or 
        customAction.Category = "MyCustomCategory";
    }
}

```

Después de agregar una acción a un controlador, puede usar el diseñador para personalizar la acción.

>NOTA
>
>[CodeRush  ](https://www.devexpress.com/products/coderush/)le permite agregar acciones y controladores con unas pocas pulsaciones de teclas. Para obtener información sobre las plantillas de código para XAF, consulte el siguiente tema de ayuda: [Plantillas XAF](https://docs.devexpress.com/CodeRushForRoslyn/403133/coding-assistance/code-templates/xaf-templates?v=22.2).

## Agregar una acción aplicando el atributo Action a un método de clase empresarial

Si necesita una  [acción](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  que se aplique a un objeto de negocio y utilice los parámetros del objeto de negocio, aplique el atributo Action al método de la clase de negocio como se muestra a continuación:

-   [Introducción: Agregar una acción sencilla mediante un atributo (.NET 6+)](https://docs.devexpress.com/eXpressAppFramework/402156/getting-started/in-depth-tutorial-blazor/add-actions-menu-commands/add-a-simple-action-using-an-attribute)
-   [Cómo: Crear una acción mediante el atributo Action](https://docs.devexpress.com/eXpressAppFramework/112619/ui-construction/controllers-and-actions/actions/how-to-create-an-action-using-the-action-attribute)

Utilice el atributo Action sólo para escenarios sencillos similares a los descritos en los artículos. Para una mayor flexibilidad, puede  [agregar una acción a un controlador](https://docs.devexpress.com/eXpressAppFramework/400495/ui-construction/controllers-and-actions/actions/perform-common-tasks-with-xaf-actions#add-an-action-to-a-controller).

## Usar la configuración de acciones

La clase base para todos los tipos de acción es la clase  [ActionBase](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase). Esta clase expone eventos, propiedades y métodos que admiten el comportamiento de  [acción](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  común.

**Eventos**

-   Los eventos ActionBase.Execution y  [ActionBase.Executed](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Executed)  se producen cuando un usuario final realiza una acción especificada: hace clic en un botón, selecciona un elemento en un cuadro combinado, etc[.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Executing)

**Propiedades**

-   [ActionBase.Controller](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Controller)  proporciona acceso al controlador principal.
-   [ActionBase.Active](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Active)  determina el estado activo/inactivo y la visibilidad de la acción.
-   [ActionBase.Enabled](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Enabled)  determina el estado activado/mostrado de una acción. Una acción deshabilitada está visible en la interfaz de usuario, pero está atenuada.
-   ActionBase.TargetViewType, ActionBase.TargetViewNesting, ActionBase.TargetViewId, ActionBase.TargetObjectType, ActionBase.TargetObjectsCriteria, ActionBase.TargetObjectsCriteriaMode y  [](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.TargetObjectsCriteria)[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.TargetViewType)[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.TargetViewId)[ActionBase.SelectionDependencyType](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.SelectionDependencyType)  especifican las condiciones para la activación  [de](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.TargetObjectsCriteriaMode)  la acción[.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.TargetViewNesting)[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.TargetObjectType)
-   [ActionBase.Category](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Category)  especifica la categoría de una acción. La categoría determina la ubicación de la acción en un contenedor de acciones.
-   [ActionBase.ConfirmationMessage](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.ConfirmationMessage)  especifica el mensaje de confirmación que se muestra cuando un usuario final ejecuta una acción.
-   [ActionBase.Caption](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Caption)  especifica el título de la acción.

>PROPINA
>
>Acceda a la página de miembros de la clase  [ActionBase](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase) para obtener una lista completa de la API disponible.

## Personalizar una acción en el modelo de aplicación

La información sobre  [acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  está disponible en el nodo  **ActionDesign**  del modelo de aplicación.

-   [Cómo: Colocar una acción en una ubicación diferente](https://docs.devexpress.com/eXpressAppFramework/402145/ui-construction/controllers-and-actions/actions/how-to-place-an-action-in-a-different-location)
-   [Cómo: Modificar propiedades de acción en el Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/402146/ui-construction/controllers-and-actions/actions/how-to-specify-action-settings)
-   [Incluir una acción en un diseño de vista detallada](https://docs.devexpress.com/eXpressAppFramework/112816/ui-construction/view-items-and-property-editors/include-an-action-to-a-detail-view-layout)

## Personalizar una acción en código

Puede acceder a  [las acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  y personalizarlas en código.

-   [Personalizar controladores y acciones](https://docs.devexpress.com/eXpressAppFramework/112676/ui-construction/controllers-and-actions/customize-controllers-and-actions)
-   [Cómo: Personalizar ASP.NET elementos de diseño de formularios Web Forms mediante clases CSS personalizadas](https://docs.devexpress.com/eXpressAppFramework/114832/application-shell-and-base-infrastructure/themes/how-to-customize-layout-elements-using-custom-css-classes/how-to-customize-asp-net-layout-elements-using-custom-css-classes)
-   [Cómo: Personalizar controles de acción](https://docs.devexpress.com/eXpressAppFramework/113183/ui-construction/controllers-and-actions/actions/how-to-customize-action-controls)
-   [Ocultar o deshabilitar una acción (botón, elemento de menú) en el código](https://docs.devexpress.com/eXpressAppFramework/112728/ui-construction/controllers-and-actions/actions/how-to-deactivate-hide-an-action-in-code)
-   [Cómo: Reordenar la colección de acciones de un contenedor de acciones](https://docs.devexpress.com/eXpressAppFramework/112815/ui-construction/controllers-and-actions/actions/how-to-reorder-an-action-containers-actions-collection)

## Crear tipos de acción personalizados y controles personalizados

En XAF, puede crear tipos de  [acción](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  personalizados y controles personalizados. Vea los ejemplos a continuación:

-   [Crear un tipo de acción personalizado con un control personalizado (BarCheckItem) asociado](https://github.com/DevExpress-Examples/XAF_how-to-create-a-custom-action-type-with-a-custom-control-barcheckitem-associated-with-it-e1977)
-   [Crear una acción personalizada con un control personalizado en una aplicación  XAF ASP.NET Web Forms](https://github.com/DevExpress-Examples/XAF_how-to-create-a-custom-action-with-a-custom-control-in-xaf-aspnet-application-e4357)

## Solucionar problemas de acciones

Consulte los artículos siguientes para obtener información sobre cómo diagnosticar y solucionar los problemas más frecuentes.

-   [Determinar por qué una acción, controlador o editor está inactivo](https://docs.devexpress.com/eXpressAppFramework/112818/ui-construction/controllers-and-actions/determine-why-an-action-controller-or-editor-is-inactive)
-   [Definir el alcance de los controladores y las acciones](https://docs.devexpress.com/eXpressAppFramework/113103/ui-construction/controllers-and-actions/define-the-scope-of-controllers-and-actions)
-   [Determinar el controlador y el identificador de una acción](https://docs.devexpress.com/eXpressAppFramework/113484/ui-construction/controllers-and-actions/determine-an-actions-controller-and-identifier)
-   [ActionBase.DiagnosticInfo](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.DiagnosticInfo)


# Acciones de acceso en diferentes áreas de la interfaz de usuario

En esta sección se proporcionan tareas específicas para acciones en diferentes áreas de la interfaz de usuario.


# Menú: Barra de herramientas principal


Puede realizar  [tareas comunes](https://docs.devexpress.com/eXpressAppFramework/400495/ui-construction/controllers-and-actions/actions/perform-common-tasks-with-xaf-actions)  con  [Acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  de la barra de herramientas principal. También puede utilizar enfoques específicos de este artículo.

## ASP.NET  Core Blazor

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/56d17fd5-a18f-435d-93a1-bbac11f92431)

**Documentación**

-   [Cómo: Crear una plantilla de aplicación de Blazor personalizada](https://docs.devexpress.com/eXpressAppFramework/403452/ui-construction/templates/in-blazor/custom-blazor-application-template)
-   [Ubicación de contenedores de acciones en la interfaz de usuario](https://docs.devexpress.com/eXpressAppFramework/112610/ui-construction/action-containers#aspnet-core-blazor-action-containers)
-   [Usar el contenedor SaveOptions](https://docs.devexpress.com/eXpressAppFramework/112610/ui-construction/action-containers#in-aspnet-core-blazor-application-template)

**Ejemplos**

-   [XAF Blazor - Implementar un tipo  de acción personalizado](https://github.com/DevExpress-Examples/xaf-custom-action-type-blazor)

## Formularios de Win

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/54f11e30-d4db-40b1-93b1-d59e34225d13)

**Documentación:**

-   [Agregar un contenedor de acciones personalizado](https://docs.devexpress.com/eXpressAppFramework/112618/ui-construction/templates/in-winforms/how-to-create-a-custom-winforms-ribbon-template)
-   [Cómo: Habilitar las barras de menú principal o la cinta de opciones en WinForms](https://docs.devexpress.com/eXpressAppFramework/404212/ui-construction/templates/in-winforms/how-to-toggle-win-forms-ribbon-interface)
-   [IModelAction.QuickAccess](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelAction.QuickAccess)
-   [SupportClientScriptsExtensions.SetClientScript](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.SupportClientScriptsExtensions.SetClientScript.overloads)

**Ejemplos:**

-   [Acceso al Administrador de documentos, al Administrador de barras y al control  de cinta](https://github.com/DevExpress-Examples/XAF_how-to-access-the-documentmanager-barmanager-and-ribboncontrol-e4027)
-   [Cómo establecer el foco en una acción parametrizada (búsqueda de texto completo) en código](https://supportcenter.devexpress.com/Ticket/Details/Q559647/how-to-set-focus-to-a-parametrizedaction-full-text-search-in-code)

## Formularios Web Forms ASP.NET

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/699441d7-b4ac-4ecd-943f-ef3b5baddada)

**Documentación:**

-   [Cómo: Personalizar una plantilla de formularios Web Forms ASP.NET](https://docs.devexpress.com/eXpressAppFramework/113460/ui-construction/templates/in-webforms/how-to-customize-an-asp-net-template#add-an-action-container-to-the-template)
-   [Uso de WebActionContainer](https://docs.devexpress.com/eXpressAppFramework/112610/ui-construction/action-containers#in-aspnet-web-forms-application-template)
-   [IModelActionWeb.AdaptivePriority](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.SystemModule.IModelActionWeb.AdaptivePriority)
-   [IModelActionWeb.CustomCSSClassName](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.SystemModule.IModelActionWeb.CustomCSSClassName)
-   [IModelActionWeb.ConfirmUnsavedChanges](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.SystemModule.IModelActionWeb.ConfirmUnsavedChanges)
-   [IModelActionWeb.IsPostBackRequired](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.SystemModule.IModelActionWeb.IsPostBackRequired)

**Ejemplos:**

-   [Cómo implementar métodos abreviados de teclado en aplicaciones  XAF ASP.NET Web Forms](https://supportcenter.devexpress.com/ticket/details/s18977/how-to-support-keyboard-shortcuts-for-actions-in-an-xaf-asp-net-webforms-and-blazor)
-   [Cómo unir varias acciones en un solo menú  desplegable](https://supportcenter.devexpress.com/Ticket/Details/T570804/how-to-join-multiple-actions-to-a-single-dropdown-menu-as-this-is-done-for-the-save-action)


# Menú: Barra de herramientas anidada


Puede realizar  [tareas comunes](https://docs.devexpress.com/eXpressAppFramework/400495/ui-construction/controllers-and-actions/actions/perform-common-tasks-with-xaf-actions)  con  [acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  anidadas de la barra de herramientas. También puede utilizar enfoques específicos de esta sección.

## Formularios de Win

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/7fb73c6b-77da-46d7-b4ad-9e22c8cd94b2)

**Documentación:**

-   [ActionBase](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase)
-   [ActionBase.TargetViewNesting](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.TargetViewNesting)
-   [ViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController)
-   [ViewController.TargetViewNesting](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController.TargetViewNesting)
-   [Ocultar la barra de herramientas DashboardViewItem](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelDashboardViewItem.ActionsToolbarVisibility)
-   [Ocultar la barra de herramientas de la vista de lista anidada](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.XafApplication.CustomizeTemplate)

**Ejemplos:**

-   [Buscar objetos dentro de ListPropertyEditor o habilitar el FillTextSearchAction estándar para un control ListView  anidado](https://supportcenter.devexpress.com/Ticket/Details/S19503/filtering-how-to-search-objects-within-listpropertyeditor-or-enabling-the-standard)
-   [Ocultar la barra de herramientas  de DetailPropertyEditor](https://supportcenter.devexpress.com/Ticket/Details/Q493232/disable-toolbar-from-detailpropertyeditor)

## Formularios Web Forms ASP.NET

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/71e01b42-5d1e-485d-9baa-db236fb7e77c)

**Documentación:**

-   [ActionBase](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase)
-   [ActionBase.TargetViewNesting](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.TargetViewNesting)
-   [ViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController)
-   [ViewController.TargetViewNesting](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController.TargetViewNesting)
-   [Ocultar la barra de herramientas DashboardViewItem](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelDashboardViewItem.ActionsToolbarVisibility)
-   [Ocultar la barra de herramientas de la vista de lista anidada](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.XafApplication.CustomizeTemplate)

**Ejemplos:**

-   [Buscar objetos dentro de ListPropertyEditor o habilitar el FillTextSearchAction estándar para un control ListView  anidado](https://supportcenter.devexpress.com/Ticket/Details/S19503/filtering-how-to-search-objects-within-listpropertyeditor-or-enabling-the-standard)
-   [Ocultar la barra de herramientas  de DetailPropertyEditor](https://supportcenter.devexpress.com/Ticket/Details/Q493232/disable-toolbar-from-detailpropertyeditor)


# Navegación

Puede realizar  [tareas comunes](https://docs.devexpress.com/eXpressAppFramework/400495/ui-construction/controllers-and-actions/actions/perform-common-tasks-with-xaf-actions)  con  [Acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  de navegación. También puede utilizar enfoques específicos de esta sección.

## Formularios de Win

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/b1b75da0-d7c5-4c60-acc9-01f251f0f892)

**Documentación:**

-   [Agregar un elemento al control de navegación](https://docs.devexpress.com/eXpressAppFramework/402131/getting-started/in-depth-tutorial-blazor/customize-navigation-between-views/add-an-item-to-navigation-control)
-   [Cambiar el estilo de los elementos de navegación](https://docs.devexpress.com/eXpressAppFramework/404199/getting-started/in-depth-tutorial-blazor/customize-navigation-between-views/change-style-of-navigation-items)
-   [Definir el estilo de control de navegación en una aplicación WinForms](https://docs.devexpress.com/eXpressAppFramework/113198/application-shell-and-base-infrastructure/navigation-system#define-the-navigation-control-style-in-a-winforms-application)
-   [Cómo: Obtener acceso al control de navegación](https://docs.devexpress.com/eXpressAppFramework/112617/application-shell-and-base-infrastructure/navigation/how-to-access-navigation-control)
-   [Cómo: Obtener acceso a la barra de navegación de Office](https://docs.devexpress.com/eXpressAppFramework/116417/application-shell-and-base-infrastructure/navigation/how-to-access-the-office-navigation-bar)
-   [IModelRootGroupsStyle.RootGroupsStyle](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.SystemModule.IModelRootGroupsStyle.RootGroupsStyle)

## Formularios Web Forms ASP.NET

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/103bda78-6f5a-4f6c-b048-84f035815792)

**Documentación:**

-   [Definir el estilo de control de navegación en una aplicación ASP.NET de formularios Web Forms](https://docs.devexpress.com/eXpressAppFramework/113198/application-shell-and-base-infrastructure/navigation-system#define-the-navigation-control-style-in-an-aspnet-web-forms-application)
-   [Cómo: Obtener acceso al control de navegación](https://docs.devexpress.com/eXpressAppFramework/112617/application-shell-and-base-infrastructure/navigation/how-to-access-navigation-control)
-   [IModelRootNavigationItems.ShowImages](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelRootNavigationItems.ShowImages)
-   [IModelRootNavigationItemsWeb.ShowNavigationOnStart](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.SystemModule.IModelRootNavigationItemsWeb.ShowNavigationOnStart)


# Ventana emergente

Puede realizar  [tareas comunes](https://docs.devexpress.com/eXpressAppFramework/400495/ui-construction/controllers-and-actions/actions/perform-common-tasks-with-xaf-actions)  con  [Acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  de ventana emergente. También puede utilizar enfoques específicos de esta sección.

## Formularios de Win

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/0c5af38a-e416-4fd3-a35a-9ea32c1357e8)

**Documentación:**

-   [Controladores y acciones de formularios de inicio de sesión](https://docs.devexpress.com/eXpressAppFramework/113475/ui-construction/controllers-and-actions/logon-form-controllers-and-actions)
-   [Agregar acciones a una ventana emergente](https://docs.devexpress.com/eXpressAppFramework/112804/ui-construction/controllers-and-actions/add-actions-to-a-popup-window)
-   [Cómo personalizar las acciones OK y Cancel](https://docs.devexpress.com/eXpressAppFramework/112805/ui-construction/controllers-and-actions/dialog-controller)

## Formularios Web Forms ASP.NET

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/b4efa5bb-c01a-4aff-9765-0a5c7411b622)

**Documentación:**

-   [Controladores y acciones de formularios de inicio de sesión](https://docs.devexpress.com/eXpressAppFramework/113475/ui-construction/controllers-and-actions/logon-form-controllers-and-actions)
-   [Agregar acciones a una ventana emergente](https://docs.devexpress.com/eXpressAppFramework/112804/ui-construction/controllers-and-actions/add-actions-to-a-popup-window)
-   [Cómo personalizar las acciones OK y Cancel](https://docs.devexpress.com/eXpressAppFramework/112805/ui-construction/controllers-and-actions/dialog-controller)
-   [PersonalizePopupWindowSizeEventArgs.ShowPopupMode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.Controls.CustomizePopupWindowSizeEventArgs.ShowPopupMode)


# Vista de lista de búsqueda

Puede realizar  [tareas comunes](https://docs.devexpress.com/eXpressAppFramework/400495/ui-construction/controllers-and-actions/actions/perform-common-tasks-with-xaf-actions)  con  [Acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  de vista de lista de búsqueda. También puede utilizar enfoques específicos de esta sección.

## Formularios de Win

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/6e53abf7-1cb4-4696-82be-306a321a336a)

### Documentación

-   [Cómo: Detectar una vista de lista de búsqueda en código](https://docs.devexpress.com/eXpressAppFramework/112908/ui-construction/ways-to-access-ui-elements-and-their-controls/how-to-detect-a-lookup-list-view-in-code)
-   [Cómo: Agregar una acción de búsqueda a editores de propiedades de búsqueda y ventanas emergentes de vínculos](https://docs.devexpress.com/eXpressAppFramework/112925/ui-construction/controllers-and-actions/actions/how-to-add-a-search-action-to-lookup-property-editors-and-link-pop-up-windows)

## Formularios Web Forms ASP.NET

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/dfe5e155-14ca-478c-b815-9b81b0e4b78b)

### Documentación

-   [Cómo: Detectar una vista de lista de búsqueda en código](https://docs.devexpress.com/eXpressAppFramework/112908/ui-construction/ways-to-access-ui-elements-and-their-controls/how-to-detect-a-lookup-list-view-in-code)
-   [Cómo: Agregar una acción de búsqueda a editores de propiedades de búsqueda y ventanas emergentes de vínculos](https://docs.devexpress.com/eXpressAppFramework/112925/ui-construction/controllers-and-actions/actions/how-to-add-a-search-action-to-lookup-property-editors-and-link-pop-up-windows)

## ASP.NET  Core Blazor

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/b059c9de-3607-4447-a39d-d4e960e27ba1)

Consulte el tema siguiente para obtener más información sobre cómo configurar las opciones de búsqueda:  [Personalizar un editor de propiedades integrado (Blazor).](https://docs.devexpress.com/eXpressAppFramework/402188/ui-construction/view-items-and-property-editors/property-editors/customize-a-built-in-property-editor-blazor)


# Vista detallada


Utilice esta sección para agregar y manipular  [acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  dentro de  [las vistas detalladas](https://docs.devexpress.com/eXpressAppFramework/112611/ui-construction/views#detailview).

## Formularios de Win

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/91702a09-dec8-4899-bf94-c1a89af80ee2)

**Documentación:**

-   [Cómo: Obtener acceso al componente de cuadrícula en una vista de lista](https://docs.devexpress.com/eXpressAppFramework/402154/ui-construction/list-editors/how-to-access-list-editor-control)
-   [Cómo: Agregar un botón a una vista detallada mediante un elemento de vista personalizada](https://docs.devexpress.com/eXpressAppFramework/113653/ui-construction/view-items-and-property-editors/add-a-button-to-a-detail-view-using-custom-view-item)
-   [Cómo: Incluir una acción en un diseño de vista detallada](https://docs.devexpress.com/eXpressAppFramework/112816/ui-construction/view-items-and-property-editors/include-an-action-to-a-detail-view-layout)

**Ejemplos:**

-   [Personalización de  acciones ActionContainerViewItem](https://supportcenter.devexpress.com/Ticket/Details/Q304471/actioncontainerviewitem-actions-customization)
-   [Cómo personalizar el alto y el ancho  de la acción de ActionContainerViewItem](https://supportcenter.devexpress.com/ticket/details/t368884/winforms-how-to-customize-actioncontainerviewitem-s-action-height-and-width)

## Formularios Web Forms ASP.NET

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/c6922583-fe70-441f-815e-87f7fdc71bfd)

**Documentación:**

-   [Cómo: Obtener acceso al componente de cuadrícula en una vista de lista](https://docs.devexpress.com/eXpressAppFramework/402154/ui-construction/list-editors/how-to-access-list-editor-control)
-   [Cómo: Agregar un botón a una vista detallada mediante un elemento de vista personalizada](https://docs.devexpress.com/eXpressAppFramework/113653/ui-construction/view-items-and-property-editors/add-a-button-to-a-detail-view-using-custom-view-item)
-   [Cómo: Incluir una acción en un diseño de vista detallada](https://docs.devexpress.com/eXpressAppFramework/112816/ui-construction/view-items-and-property-editors/include-an-action-to-a-detail-view-layout)

**Ejemplos**:

-   [Cómo personalizar el alto y el ancho  de la acción de ActionContainerViewItem](https://supportcenter.devexpress.com/ticket/details/q470346/web-how-to-customize-actioncontainerviewitem-s-action-height-and-width)
-   [Cómo limitar el ancho de WebActionContainer a su ancho  de control primario](https://supportcenter.devexpress.com/Ticket/Details/T299059/how-to-limit-webactioncontainer-s-width-to-its-parent-control-width)

## ASP.NET  Core Blazor

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/3ebd188f-d49e-4b27-867c-b68999e8e292)

**Documentación:**

-   [Configuración del editor de acceso](https://docs.devexpress.com/eXpressAppFramework/402153/getting-started/in-depth-tutorial-blazor/customize-data-display-and-view-layout/access-editor-settings)
-   [Cómo: Incluir una acción en un diseño de vista detallada](https://docs.devexpress.com/eXpressAppFramework/112816/ui-construction/view-items-and-property-editors/include-an-action-to-a-detail-view-layout)


# Otros menús


Puede realizar  [tareas comunes](https://docs.devexpress.com/eXpressAppFramework/400495/ui-construction/controllers-and-actions/actions/perform-common-tasks-with-xaf-actions)  con  [Acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  dentro de Menús. También puede utilizar enfoques específicos de esta sección.

## Formularios de Win

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/d726c2d1-1340-4508-be36-ce42f186ec7e)

**Documentación:**

-   [Cómo agregar una acción a un menú contextual](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Category#how-to-add-an-action-to-a-context-menu-or-command-column)
-   [Cómo: Reemplazar la acción predeterminada de una vista de lista](https://docs.devexpress.com/eXpressAppFramework/112820/ui-construction/controllers-and-actions/actions/how-to-replace-a-list-views-default-action)

## Formularios Web Forms ASP.NET

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/6e8caa3c-f6f3-4f20-a72e-b8666e848e20)

**Documentación:**

-   [Cómo agregar una acción a una columna de comandos](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Category)
-   [Cómo: Ocultar la columna Editar acción de un control ListView en una aplicación de formularios Web Forms ASP.NET](https://docs.devexpress.com/eXpressAppFramework/114715/ui-construction/controllers-and-actions/actions/how-to-hide-the-edit-action-column-from-a-list-view-in-an-asp-net-application)
-   [Cómo: Reemplazar la acción predeterminada de una vista de lista](https://docs.devexpress.com/eXpressAppFramework/112820/ui-construction/controllers-and-actions/actions/how-to-replace-a-list-views-default-action)

**Ejemplos:**

-   [Agregar una acción en línea a la fila  Control de vista de lista](https://supportcenter.devexpress.com/Ticket/Details/K18108/how-to-provide-an-inline-action-shown-right-within-the-listview-control-row-on-the-web)
-   [Ocultar un botón de edición en línea de una vista  de lista](https://supportcenter.devexpress.com/ticket/details/t548509/web-how-to-hide-the-inline-edit-button-from-a-listview)


# Cómo: Personalizar controles de acción


En este ejemplo se muestra cómo personalizar el control que visualiza una  [acción](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  en una interfaz de usuario. Se creará una acción personalizada, que permitirá a los usuarios introducir una fecha y filtrar la vista de lista en consecuencia. La acción implementada aceptará la entrada del teclado y proporcionará un calendario desplegable. El control que representa la acción se personalizará para aceptar la entrada del teclado mediante una máscara personalizada. La siguiente imagen muestra la acción resultante en una interfaz de usuario.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/4519c118-f84d-4f1a-af3e-90f129f26d78)

>NOTA
>
>Puede ver una demostración de los siguientes controladores (**Personalizarel controlador de acciones de menúpara formularios Web Forms ASP.NET y Personalizar controlador de acciones**  **parametrizadopara formularios Win) en la sección Acciones del Centro de características**  aplicación que se incluye con XAF. La ubicación predeterminada de la aplicación es %_PUBLIC%\Documents\DevExpress Demos 23.1\Componentes\XAF\Centro de características.NETFramework.  XPO._

En primer lugar, defina la clase de negocio  **MyDomainObject**  de ejemplo. La clase contiene dos propiedades. La primera es la propiedad "CreatedOn" del tipo  **DateTime**  y la segunda es la propiedad "Title" del tipo  **de cadena**.

**EF Core**

```csharp
using DevExpress.Persistent.Base;
using DevExpress.Persistent.BaseImpl.EF;
// ...
[DefaultClassOptions]
public class MyDomainObject : BaseObject {
    public virtual DateTime CreatedOn { get; set; }
    public virtual string Title { get; set; }
}

// Make sure that you use options.UseChangeTrackingProxies() in your DbContext settings.

```
**XPO**

```csharp
using DevExpress.Persistent.Base;
using DevExpress.Persistent.BaseImpl;
using DevExpress.Xpo;
// ...
[DefaultClassOptions]
public class MyDomainObject : BaseObject {
    public MyDomainObject(Session session) : base(session) { }

    public DateTime CreatedOn {
        get { return GetPropertyValue<DateTime>(nameof(CreatedOn)); }
        set { SetPropertyValue(nameof(CreatedOn), value); }
    }

    public string Title {
        get { return GetPropertyValue<string>(nameof(Title)); }
        set { SetPropertyValue(nameof(Title), value); }
    }
}
```
A continuación, cree un nuevo  [controlador de vistas](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers)  y defina una  [acción parametrizada](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ParametrizedAction). Configure el controlador y la acción para que se activen solo para la clase  **MyDomainObject**. Establezca  [ParametrizedAction.ValueType](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ParametrizedAction.ValueType)  de la acción en  **DateTime**. En el controlador de eventos  [ParametrizedAction.Execute](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ParametrizedAction.Execute)  de la acción, compruebe si se ha introducido una fecha. Si se ha introducido una fecha, filtre la vista de lista  **MyDomainObject**  para que contenga sólo los objetos cuyo valor de la propiedad "CreatedOn" sea igual a la fecha especificada. Si un usuario ha ejecutado la Acción con un campo de edición vacío, elimine el filtro aplicado, de modo que la Vista de lista muestre todos los objetos, independientemente de su fecha de creación.



```csharp
using DevExpress.Data.Filtering;
using DevExpress.ExpressApp;
using DevExpress.ExpressApp.Actions;
using DevExpress.Persistent.Base;
//...
public class MyFilterController : ViewController {
    public ParametrizedAction dateFilterAction;
    public MyFilterController() {
        TargetViewType = ViewType.ListView;
        TargetObjectType = typeof(MyDomainObject);
        dateFilterAction = new ParametrizedAction(this, "MyDateFilter", PredefinedCategory.Search, typeof(DateTime));
        dateFilterAction.NullValuePrompt = "Enter date";
        dateFilterAction.Execute += dateFilterAction_Execute;
    }
    private void dateFilterAction_Execute(object sender, ParametrizedActionExecuteEventArgs e) {
        CriteriaOperator criterion = null;
        if(e.ParameterCurrentValue != null && e.ParameterCurrentValue.ToString() != string.Empty) {
            criterion = new BinaryOperator("CreatedOn", Convert.ToDateTime(e.ParameterCurrentValue));
        }
        ((ListView)View).CollectionSource.Criteria[dateFilterAction.Id] = criterion;
    }
}

```

Para configurar una máscara de edición personalizada, suscríbase al evento  [ActionBase.CustomizeControl.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.CustomizeControl)  Este evento permite personalizar el control de elementos creados y proporciona acceso al control de acciones correspondiente.

## Formularios de Win


```csharp
using DevExpress.ExpressApp;
using DevExpress.ExpressApp.Actions;
using DevExpress.XtraBars;
using DevExpress.XtraEditors.Repository;
//...
public class CustomizeActionControlControllerWin : Controller {
    protected override void OnActivated() {
        base.OnActivated();
        Frame.GetController<MyFilterController>().dateFilterAction.CustomizeControl += CustomizeActionControlControllerWin_CustomizeControl;
    }
    private void CustomizeActionControlControllerWin_CustomizeControl(object sender, 
CustomizeControlEventArgs e) {
        BarEditItem barItem = e.Control as BarEditItem;
        if (barItem != null) {
            RepositoryItemDateEdit repositoryItem = (RepositoryItemDateEdit)barItem.Edit;
            repositoryItem.Mask.UseMaskAsDisplayFormat = true;
            repositoryItem.Mask.EditMask = "yyyy-MMM-dd";
            barItem.Width = 270;
        }
    }
    protected override void OnDeactivated() {
        Frame.GetController<MyFilterController>().dateFilterAction.CustomizeControl -= 
CustomizeActionControlControllerWin_CustomizeControl;
        base.OnDeactivated();
    }
}

```

## Formularios Web Forms ASP.NET

Este enfoque utiliza las propiedades del servidor. Emplee este enfoque cuando necesite cambiar el comportamiento de un control. Si necesita cambiar la apariencia de un control, use estilos CSS como se describe en el artículo  [Personalización de acciones](https://docs.devexpress.com/eXpressAppFramework/114832/application-shell-and-base-infrastructure/themes/how-to-customize-layout-elements-using-custom-css-classes/how-to-customize-asp-net-layout-elements-using-custom-css-classes#4).



```csharp
using DevExpress.ExpressApp;
using DevExpress.ExpressApp.Actions;
using DevExpress.ExpressApp.Web.Templates.ActionContainers.Menu;
using DevExpress.Web;
//...
public class CustomizeActionControlControllerWeb : Controller {
    protected override void OnActivated() {
        base.OnActivated();
        Frame.GetController<MyFilterController>().dateFilterAction.CustomizeControl += CustomizeActionControlControllerWeb_CustomizeControl;
    }
    private void CustomizeActionControlControllerWeb_CustomizeControl(object sender, 
CustomizeControlEventArgs e) {
        ParametrizedActionMenuActionItem actionItem = e.Control as ParametrizedActionMenuActionItem;
        if (actionItem != null) {
            ASPxDateEdit editor = actionItem.Control.Editor as ASPxDateEdit;
            if (editor != null) {
                editor.UseMaskBehavior = true;
                editor.EditFormat = EditFormat.DateTime;
                editor.EditFormatString = "yyyy-MMM-dd";
                editor.Width = 270;
            }
        }
    }
    protected override void OnDeactivated() {
        Frame.GetController<MyFilterController>().dateFilterAction.CustomizeControl -= 
CustomizeActionControlControllerWeb_CustomizeControl;
        base.OnDeactivated();
    }
}

```

## ASP.NET Core Blazor

Add the following Controller to the ASP.NET Core Blazor  [module project](https://docs.devexpress.com/eXpressAppFramework/118045/application-shell-and-base-infrastructure/application-solution-components/application-solution-structure)  (_MySolution.Module.Blazor_). If your solution does not contain this project, add this Controller to the  [application project](https://docs.devexpress.com/eXpressAppFramework/118045/application-shell-and-base-infrastructure/application-solution-components/application-solution-structure)  (_MySolution.Blazor.Server_).



```csharp
using MySolution.Module.Controllers;
using DevExpress.ExpressApp.Blazor.Components.Models;
using DevExpress.ExpressApp.Blazor.Templates.Toolbar.ActionControls;
// ...

public class CustomizeActionControlControllerBlazor : Controller {
    private MyFilterController myFilterController;
    protected override void OnActivated() {
        base.OnActivated();
        myFilterController = Frame.GetController<MyFilterController>();
        if(myFilterController != null) {
            myFilterController.dateFilterAction.CustomizeControl += 
                CustomizeActionControlController_CustomizeControl;
        }
    }
    private void CustomizeActionControlController_CustomizeControl(object sender, 
        CustomizeControlEventArgs e) {
        if(e.Control is DxToolbarItemParametrizedActionControl actionControl && 
            actionControl.EditModel is DxDateEditModel dateEditModel) {
            dateEditModel.Format = "yyyy-MMM-dd";
        }
    }
    protected override void OnDeactivated() {
        if(myFilterController != null) {
            myFilterController.dateFilterAction.CustomizeControl -= 
            CustomizeActionControlController_CustomizeControl;
            myFilterController = null;
        }
        base.OnDeactivated();
    }
}

```

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/27ac5723-4d89-46fc-8e5a-8786b1ee2a61)


# Cómo: Crear una acción mediante el atributo Action


En este ejemplo se muestra cómo crear una  [acción](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  dentro de una declaración de clase persistente (es decir, cómo convertir un método de clase persistente en  [SimpleAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.SimpleAction)  o  [PopupWindowShowAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.PopupWindowShowAction)).

## Notas importantes sobre el uso del atributo action

Las acciones creadas con  [ActionAttribute](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.ActionAttribute)  no están diseñadas para funcionar en vistas de lista sin ningún objeto seleccionado. El parámetro  [ActionAttribute.SelectionDependencyType](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.ActionAttribute.SelectionDependencyType)  del atributo se puede establecer en  **RequireSingleObject**  o  **RequireMultipleObjects**  (vea  [MethodActionSelectionDependencyType](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.MethodActionSelectionDependencyType)). Para crear una acción que no requiera el objeto seleccionado, agregue un  [controlador](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers)  e implemente una acción en él (consulte Agregar una acción  [simple y Agregar](https://docs.devexpress.com/eXpressAppFramework/402157/getting-started/in-depth-tutorial-blazor/add-actions-menu-commands/add-a-simple-action)  una  [acción que muestre una ventana emergente](https://docs.devexpress.com/eXpressAppFramework/402158/getting-started/in-depth-tutorial-blazor/add-actions-menu-commands/add-an-action-that-displays-a-pop-up-window)).

[ActionAttribute](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.ActionAttribute)  se utiliza principalmente en escenarios sencillos sólo cuando se ejecuta lógica basada en datos disponibles en el contexto de clase empresarial (por ejemplo, modificar propiedades de clase).  [ActionAttribute](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.ActionAttribute)  NO es adecuado para ninguna lógica relacionada con la interfaz de usuario ni para interacciones de usuario complejas. Tampoco es posible acceder a  [XafApplication](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.XafApplication),  [Views](https://docs.devexpress.com/eXpressAppFramework/112611/ui-construction/views)  y otras entidades relacionadas con la interfaz de usuario de XAF dentro del código de clase empresarial, ya que viola el principio de  [separación de preocupaciones](https://en.wikipedia.org/wiki/Separation_of_concerns) y va en contra de la arquitectura  [MVC](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) (su modelo de datos no debe estar vinculado a la interfaz de usuario). Para acceder a  [XafApplication](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.XafApplication),  [Views](https://docs.devexpress.com/eXpressAppFramework/112611/ui-construction/views)  y otras entidades relacionadas con la interfaz de usuario, implemente  [Controllers](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers)  with  [Actions](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions).

## Crear una acción simple

>PROPINA
>
>Un proyecto de ejemplo completo está disponible en la base de datos de ejemplos de código de DevExpress en [https://supportcenter.Devexpress.  com/ticket/details/e3884/how-to-create-an-action-using-the-actionattribute](https://supportcenter.devexpress.com/ticket/details/e3884/how-to-create-an-action-using-the-actionattribute) .

Comencemos con la siguiente clase de negocios  **de tareas**.

**EF Core**

```csharp
[DefaultClassOptions,ImageName("BO_Task"),DefaultProperty(nameof(Subject))]
public class Task : BaseObject {
    public virtual string Subject { get; set; }
    public virtual bool IsCompleted { get; set; }
}

// Make sure that you use options.UseChangeTrackingProxies() in your DbContext settings.

```

**XPO**
```
[DefaultClassOptions, ImageName("BO_Task"), DefaultProperty(nameof(Subject))]
public class Task : BaseObject {
    public Task(Session session) : base(session) { }
    private string subject;
    public string Subject {
        get { return subject; }
        set { SetPropertyValue(nameof(Subject), ref subject, value); }
    }
    private bool isCompleted;
    public bool IsCompleted {
        get { return isCompleted; }
        set { SetPropertyValue(nameof(IsCompleted), ref isCompleted, value); }
    }
}
```
Suponga que es necesario implementar una acción que cambie el valor  **IsCompleted**  a  **true**. Agregue el siguiente método  **Complete**  a la clase  **Task**  y decórelo con el atributo  [ActionAttribute](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.ActionAttribute).



```csharp
[Action(Caption="Complete", TargetObjectsCriteria = "Not [IsCompleted]")]
public void Complete() {
    IsCompleted = true;
}

```

**ObjectMethodActionsViewController**  recopila automáticamente estos métodos de las clases de negocio. Este controlador creará la acción  **Task.Complete**, que invocará el método  **Complete**  para cada  **tarea**  seleccionada. Tenga en cuenta el uso del parámetro  [ActionAttribute.TargetObjectsCriteria.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.ActionAttribute.TargetObjectsCriteria)  La acción se deshabilitará para  **las tareas**  que ya se han completado (la propiedad  **IsCompleted**  es  **true**). Puede pasar otros parámetros para personalizar el aspecto y el comportamiento de la acción. Las imágenes siguientes ilustran la acción  **completa**  en la interfaz de usuario.

**WinForms**

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/62e3e6c6-86d8-4a61-b238-0aaf7ae6b743)

**ASP.NET formularios web**

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/5eb49323-2b61-47ff-b3f1-25e50777f2c2)

**Blazor**

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/be99f295-c869-415d-babf-40e478889f72)

## Crear una acción que muestre el cuadro de diálogo emergente

A continuación, amplíe la clase  **Task**  mostrada en la sección anterior de este tema con varias propiedades adicionales para demostrar un escenario más complejo. Aquí están estas propiedades (**Fecha límite**  y  **Comentarios**).

**EF Core**

```csharp
using DevExpress.ExpressApp.Model;
// ...
public virtual DateTime? Deadline { get; set; }

[FieldSize(FieldSizeAttribute.Unlimited),ModelDefault("AllowEdit", "False")]
public virtual string Comments { get; set; }

// Make sure that you use options.UseChangeTrackingProxies() in your DbContext settings.
```
**XPO**
```csharp
using DevExpress.ExpressApp.Model;
// ...
public virtual DateTime? Deadline { get; set; }

[FieldSize(FieldSizeAttribute.Unlimited),ModelDefault("AllowEdit", "False")]
public virtual string Comments { get; set; }

// Make sure that you use options.UseChangeTrackingProxies() in your DbContext settings.

```

Suponga que debe implementar una acción que posponga la  **fecha límite**  durante un número específico de días y actualice el texto de  **los comentarios**. En primer lugar,  [implemente la siguiente clase no persistente](https://docs.devexpress.com/eXpressAppFramework/116516/business-model-design-orm/non-persistent-objects)  que declara los parámetros que se especificarán cuando un usuario final posponga una  **tarea**.



```csharp
using DevExpress.ExpressApp.DC;
// ...
[DomainComponent]
public class PostponeParametersObject {
    public PostponeParametersObject() { PostponeForDays = 1; }
    public uint PostponeForDays { get; set; }
    [FieldSize(FieldSizeAttribute.Unlimited)]
    public string Comment { get; set; }
}

```

A continuación, agregue el siguiente método  **Postpone**  a la clase  **Task**. Este método toma un parámetro del tipo  **PostponeParametersObject**  y se decora con el atributo  **Action**.



```csharp
[Action(Caption = "Postpone",
    TargetObjectsCriteria = "[Deadline] Is Not Null And Not [IsCompleted]")]
public void Postpone(PostponeParametersObject parameters) {
    if (Deadline.HasValue && !IsCompleted && (parameters.PostponeForDays > 0)) {
        Deadline += TimeSpan.FromDays(parameters.PostponeForDays);
        Comments += String.Format("Postponed for {0} days, new deadline is {1:d}\r\n{2}\r\n",
        parameters.PostponeForDays, Deadline, parameters.Comment);
    }
}

```

Como resultado, la acción  **Task.Postpon**, que acompaña a las vistas de  **tareas**, se creará automáticamente. Esta acción invoca un cuadro de diálogo con la vista de detalles  **PostponeParametersObject**  y ejecuta el método  **Postpone**  después de que un usuario final especifique los parámetros y haga clic en  **Aceptar**. Las siguientes imágenes ilustran esto.

**WinForms**

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/3ab517b3-5737-4d72-b61b-814ad54efa2c)

**ASP.NET formularios web**

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/3657466a-da80-4c2b-a49e-de995854283e)

**Blazor**

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/a765df1c-2e89-4e95-af12-299f5829af47)

>PROPINA
>
>Si es necesario reordenar los parámetros que se muestran en un cuadro de diálogo emergente, modifique el diseño de la Vista detallada de un objeto de parámetro en el [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor). En el ejemplo anterior, el nodo Vista detallada apropiado es **PosponerparámetrosObject_DetailVista**. Consulte el tema Personalización del diseño de elementos de vista  para obtener más información sobre las personalizaciones  [de diseño](https://docs.devexpress.com/eXpressAppFramework/112817/ui-construction/views/layout/view-items-layout-customization).

### Acceso a la instancia de objeto actual

Si declara un constructor  **PostponeParametersObject**  que toma un parámetro del tipo de objetos de negocio de destino (Task), la instancia  de  **Task**  actual se pasará a este constructor cuando se ejecute la acción.



```csharp
[DomainComponent]
public class PostponeParametersObject {
    private Task task;
    public PostponeParametersObject(Task task) {
        PostponeForDays = 1;
        this.task = task;
    }
    // ...
}
```


# Cómo: Agregar una acción de búsqueda a editores de propiedades de búsqueda y ventanas emergentes de vínculos


Si ya ha ejecutado una aplicación de formularios Windows Forms o ASP.NET Web Forms, es posible que observe que los editores de propiedades de búsqueda, que muestran propiedades de referencia, contienen una lista de objetos existentes del tipo especificado en el menú desplegable. Del mismo modo, la ventana emergente de la acción de vínculo muestra una lista de objetos disponibles del tipo especificado. La siguiente imagen muestra tanto la ventana desplegable del Editor de propiedades de búsqueda como la ventana emergente de la Acción de vínculo:

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/b8930d67-bcfd-4842-8dd1-50f895f8a1ee)

Sin embargo, este enfoque para mostrar simultáneamente todos los objetos, no es apropiado cuando hay muchos objetos para mostrar. Por lo tanto,  **eXpressApp Framework**  proporciona una funcionalidad de  **búsqueda**  incorporada. En este tema se detalla cómo activarlo.

La funcionalidad de búsqueda se activa en WinForms y ASP.NET aplicaciones de formularios Web Forms cuando una vista de lista en un editor de propiedades de búsqueda o una ventana emergente de acción de vínculo contiene más  **de**  25 objetos de forma predeterminada. Puede cambiar este número. Para ello, utilice la propiedad  [IModelOptions.LookupSmallCollectionItemCount](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelOptions.LookupSmallCollectionItemCount)  del nodo  **Opciones**  del  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works).

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/3dee6031-833c-400e-b68c-917a24d850c9)

Esta propiedad especifica el recuento mínimo de objetos que deben estar en una vista de lista para activar la funcionalidad de búsqueda.

En ASP.NET aplicaciones Core Blazor, la funcionalidad de búsqueda se activa en todas las ventanas emergentes de Acción de vínculo y se deshabilita en todos los editores de propiedades de  **búsqueda**.

La funcionalidad  **de búsqueda**  es proporcionada por un editor y el botón  **Buscar**:

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/02b81d10-dd36-4866-9e56-f9e8b2363552)

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/60525129-cb6d-448a-ba18-287b8f7de8d3)

Este botón ejecuta la acción  [FilterController.FullTextFilterAction de FilterController.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.FilterController.FullTextFilterAction)  [](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.FilterController)Esta acción filtra el origen de la colección de la vista de lista, buscando los objetos cuya representación de cadena de propiedades incluye el valor especificado por un usuario final. Las propiedades incluyen las que se enumeran en  **Vistas**  |  **_<ListView>_**  nodo que define la vista de lista, sin importar si es visible o invisible, persistente o no persistente. Para obtener más información, consulte la descripción de la clase  [FilterController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.FilterController).

Puede tener la funcionalidad de  **búsqueda**  disponible en el Editor de propiedades de búsqueda o en la ventana emergente de la acción de vínculo cuando haya cualquier número de objetos contenidos en el origen de la colección de la vista de lista. Para ello, aplique  [LookupEditorModeAttribute](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.LookupEditorModeAttribute)  a la propiedad requerida (una referencia de la propiedad collection). Con este atributo, puede establecer uno de los siguientes modos para el Editor de propiedades de búsqueda correspondiente o la ventana emergente de Acción de vínculo:

-   **Automático**
    
    La característica  **Buscar**  se agrega si el recuento de objetos presunto de su colección de orígenes de datos es mayor que el valor del atributo  **LookupSmallCollectionItemCount**.
    
-   **AllItems**
    
    Se cargan todos los objetos del tipo especificado.
    
-   **Buscar**
    
    No se carga ninguno de los objetos existentes del tipo especificado y la característica  **Buscar**  está disponible.
    
-   **AllItemsWithSearch**
    
    Se cargan todos los objetos del tipo especificado y la función  **Buscar**  está disponible.
    

Para establecer el modo requerido, pase el valor correspondiente como parámetro  **de LookupEditorModeAttribute**. Como alternativa, puede utilizar el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor). El valor del  _parámetro mode_  del atributo LookupEditorMode se establece para la propiedad  **LookupEditorMode**  de  **BOModel**  |  **_<Clase>_**  |  **Miembros Propios**  |  **_<Miembro>_**  nodo.

Si no se utiliza el atributo  **LookupEditorModeAttribute**, el Editor de propiedades de búsqueda se muestra en el modo  **Auto**.


# Cómo: Agregar la acción Analizar a vistas de lista


Para agregar la capacidad de analizar datos en su aplicación,  **eXpressApp Framework**  proporciona el módulo de  [gráfico dinámico](https://docs.devexpress.com/eXpressAppFramework/113024/analytics/pivot-chart-module). El tema  [Analizar datos](https://docs.devexpress.com/eXpressAppFramework/404213/analytics/pivot-chart/how-to-analyze-data)  explica que para iniciar la funcionalidad Análisis en una aplicación, debe agregar este módulo y la clase empresarial de Análisis integrada a la aplicación. En este caso, el control de navegación contendrá el elemento Análisis y el usuario final podrá crear objetos Análisis. Sin embargo, es posible que deba proporcionar la capacidad de crear un objeto Analysis desde cualquier vista de lista, estableciendo la propiedad  **DataType**  del nuevo objeto Analysis en el tipo de objeto de la vista de lista. En este tema se muestra cómo realizar esta tarea.

>NOTA
>
>Las aplicaciones ASP.NET Core Blazor no admiten el  [módulo de gráfico dinámico](https://docs.devexpress.com/eXpressAppFramework/113024/analytics/pivot-chart-module), por lo que el enfoque descrito en este tema no se puede implementar en la plataforma ASP.NET Core Blazor.

>PROPINA
>
>Un proyecto de ejemplo completo está disponible en la base de datos de ejemplos de código de DevExpress en [https://supportcenter.Devexpress.  com/ticket/details/e389/how-to-add-the-analyze-action-to-list-views](https://supportcenter.devexpress.com/ticket/details/e389/how-to-add-the-analyze-action-to-list-views) .

-   Cree un nuevo  [ViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController)  en el  [proyecto de módulo](https://docs.devexpress.com/eXpressAppFramework/118045/application-shell-and-base-infrastructure/application-solution-components/application-solution-structure)  de aplicación. Si la solución no contiene este proyecto, cree este Controller en un  [proyecto de aplicación](https://docs.devexpress.com/eXpressAppFramework/118045/application-shell-and-base-infrastructure/application-solution-components/application-solution-structure). Invoque al  **diseñador**  y establezca la propiedad  [ViewController.TargetViewType en ListView.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController.TargetViewType)  Esto significa que el controlador se activará solo para vistas de lista.
    
    Para obtener más información sobre la creación del controlador, consulte la sección  [Agregar acciones (comandos de menú)](https://docs.devexpress.com/eXpressAppFramework/402128/getting-started/in-depth-tutorial-blazor/add-actions-menu-commands)  en el Tutorial.
    
-   En el Diseñador del controlador, agregue una acción simple (consulte  [Agregar una acción simple](https://docs.devexpress.com/eXpressAppFramework/402157/getting-started/in-depth-tutorial-blazor/add-actions-menu-commands/add-a-simple-action)). Establezca su propiedad  [ActionBase.Id](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Id)  en  **AnalyzeData**, la propiedad ActionBase.Category en  **RecordEdit**, la propiedad ActionBase.Caption en  **Analyze**  y la propiedad  [ActionBase.ImageName](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.ImageName)  en  **BO_Analysis**  (una imagen integrada).[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Category)[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Caption)
-   Para desactivar la acción cuando  [VisibleInReportsAttribute](https://docs.devexpress.com/eXpressAppFramework/DevExpress.Persistent.Base.VisibleInReportsAttribute)  no se aplica a la clase empresarial representada por la vista de lista actual, suscríbase al evento  [Controller.Activated](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Controller.Activated)  del Controller. Controle este evento de la siguiente manera:
    

    
    ```csharp
    using DevExpress.ExpressApp.Model;
    // ...
    public partial class AnalyseDataFromAnyViewController : ViewController {
        //...
        private void AnalyseDataFromAnyViewController_Activated(object sender, EventArgs e) {
            analyseDataAction.Active["VisibleInReports"] =
                ((IModelClassReportsVisibility)View.Model.Application.BOModel.GetClass(
                    View.ObjectTypeInfo.Type)).IsVisibleInReports;
        }
    }
    
    ```
    
    Para desactivar la acción, se utiliza su propiedad  [ActionBase.Active.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Active)
    
-   Suscríbase al evento  [SimpleAction.Execute](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.SimpleAction.Execute)  de la acción para ejecutar el código que invocará una vista detallada con un objeto Analysis recién creado. Controle este evento de la siguiente manera:
    

    ```csharp
    using DevExpress.Persistent.BaseImpl;
    //...
    public partial class AnalyseDataFromAnyViewController : ViewController {
       //...
       private void analyseDataAction_Execute(object sender, SimpleActionExecuteEventArgs e) {
          IObjectSpace objectSpaceInternal = Application.CreateObjectSpace(typeof(Analysis));
          Analysis obj = objectSpaceInternal.CreateObject<Analysis>();
          obj.DataType = View.ObjectTypeInfo.Type;
          obj.Name = "Analysis: " + View.Caption + " " + DateTime.Now.ToString();
          e.ShowViewParameters.CreatedView = Application.CreateDetailView(objectSpaceInternal, obj);
          e.ShowViewParameters.TargetWindow = TargetWindow.Default;
          e.ShowViewParameters.Context = TemplateContext.View;
       }
    }
    
    ```
    
    En el código anterior, se utiliza el objeto  [ShowViewParameters](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ShowViewParameters). Para obtener más información sobre cómo utilizar objetos de este tipo, consulte el tema  [Formas de mostrar una vista](https://docs.devexpress.com/eXpressAppFramework/112803/ui-construction/views/ways-to-show-a-view/ways-to-show-a-view).
    
-   Dado que el objeto Analysis se creará con las propiedades  **Name**  y  **DataType**  de configuración, es apropiado enlazar datos a la vez. De lo contrario, un usuario final tendrá que realizar esta acción adicional él mismo. Para enlazar datos, ejecute  **BindAnalysisData**  (![BindAnalysisData](https://docs.devexpress.com/eXpressAppFramework/images/bindanalysisdata116076.png)) en código. Para ello, implemente un descendiente de  **AnalysisDataBindController,**  tal como se detalla en el tema  [Data Bind Aspects](https://docs.devexpress.com/eXpressAppFramework/113048/analytics/pivot-chart/data-bind-aspects).
    

    
    ```csharp
    using DevExpress.ExpressApp.PivotChart;
    //...
    public class AssignAnalysisDataSourceViewController : AnalysisDataBindController {
       protected override void OnActivated() {
          base.OnActivated();
          Analysis obj = View.CurrentObject as Analysis;
          //Allow data source loading if the ObjectTypeName property is specified
          if(obj.ObjectTypeName != null) {
             analysisEditor.IsDataSourceReady = true;
             UpdateBindUnbindActionsState();
          }
       }
    }
    
    ```
    
    >NOTA
    >
    >Tenga en cuenta que debe tener el _DevExpress.Gráfico dinámico de ExpressApp.Pivot.  v23.1_ ensamblaje al que se hace referencia en el proyecto con el Controller.
    

En la siguiente imagen se muestra la acción  **AnalyzeData**  que está disponible para una vista de lista de contactos. Al hacer clic en esta acción, se muestra un objeto Análisis recién creado en una vista de detalles invocada. Las propiedades  **Name**  y  **DataType**  del objeto se especifican automáticamente.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/b39b4bbd-fa88-43a9-8d7f-af08dbe146ec)


# Cómo: Personalizar la lista de elementos de la nueva acción


En este tema se muestra cómo tener acceso a la lista de  [clases de negocio](https://docs.devexpress.com/eXpressAppFramework/113664/business-model-design-orm)  agregadas a la lista de elementos  [NewObjectViewController.NewObjectAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController.NewObjectAction)  en WinForms y ASP.NET aplicaciones de formularios Web Forms con la  [interfaz de usuario web clásica](https://docs.devexpress.com/eXpressAppFramework/113153/application-shell-and-base-infrastructure/themes/asp-net-web-application-appearance).

Por lo general, puede utilizar la propiedad  [NewObjectViewController.NewObjectActionItemListMode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController.NewObjectActionItemListMode)  para elegir el modo predefinido de rellenar la lista de elementos Nueva acción. Si los modos enumerados en la enumeración  [NewObjectActionItemListMode](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectActionItemListMode)  no se ajustan a sus requisitos, consulte cómo rellenar la lista manualmente.

Para personalizar la lista Elementos de la nueva acción, controle los eventos NewObjectViewController.CollectDescendantTypes y  [NewObjectViewController.CollectCreatableItemTypes](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController.CollectCreatableItemTypes)  de  [NewObjectViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController), que contiene la  **nueva**  acción[.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController.CollectDescendantTypes)  El primer evento se genera cuando el tipo de objeto actual y sus descendientes se agregan a la lista Elementos de acción, y el segundo se genera cuando se agregan todos los tipos restantes cuya propiedad  **CreatableItem**  se establece en  **true**  en el modelo de aplicación (vea  [IModelBOModel](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelBOModel)). En el ejemplo siguiente, se quita el elemento  **Tarea**.

Tenga en cuenta que en ASP.NET aplicaciones Core Blazor y ASP.NET Web Forms con la  [nueva interfaz de usuario web](https://docs.devexpress.com/eXpressAppFramework/113153/application-shell-and-base-infrastructure/themes/asp-net-web-application-appearance), este controlador oculta la  **nueva**  acción de las vistas de  **tareas**.

>NOTA
>
>Un proyecto de muestra completo está disponible en [https://github.com/DevExpress-Examples/XAF_how-to-customize-the-new-actions-items-list-e238](https://github.com/DevExpress-Examples/XAF_how-to-customize-the-new-actions-items-list-e238) .

-   [PersonalizeNewActionItemsListController.cs](https://docs.devexpress.com/eXpressAppFramework/112915/ui-construction/controllers-and-actions/actions/how-to-customize-the-new-actions-items-list#tabpanel_G+uVdswYOE_tabid-csharpCustomizeNewActionItemsListController-cs)
-   [PersonalizeNewActionItemsListController.vb](https://docs.devexpress.com/eXpressAppFramework/112915/ui-construction/controllers-and-actions/actions/how-to-customize-the-new-actions-items-list#tabpanel_G+uVdswYOE_tabid-vbCustomizeNewActionItemsListController-vb)

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using DevExpress.ExpressApp;
using DevExpress.Persistent.BaseImpl;
using DevExpress.ExpressApp.SystemModule;

namespace CustomizeNewActionItemsListExample.Module.Controllers {
    public class CustomizeNewActionItemsListController : ObjectViewController<ObjectView, Task> {
        protected override void OnActivated() {
            base.OnActivated();
            NewObjectViewController controller = Frame.GetController<NewObjectViewController>();
            if (controller != null) {
                controller.CollectCreatableItemTypes += NewObjectViewController_CollectCreatableItemTypes;
                controller.CollectDescendantTypes += NewObjectViewController_CollectDescendantTypes;
                if (controller.Active) {
                    controller.UpdateNewObjectAction();
                }
            }
        }
        private void NewObjectViewController_CollectDescendantTypes(object sender, CollectTypesEventArgs e) {
            CustomizeList(e.Types);
        }
        private void NewObjectViewController_CollectCreatableItemTypes(object sender, CollectTypesEventArgs e) {
            CustomizeList(e.Types);
        }
        private void CustomizeList(ICollection<Type> types) {
            List<Type> unusableTypes = new List<Type>();
            foreach (Type item in types) {
                if (item == typeof(Task)) {
                    unusableTypes.Add(item);
                }
            }
            foreach (Type item in unusableTypes) {
                types.Remove(item);
            }
        }
        protected override void OnDeactivated() {
            NewObjectViewController controller = Frame.GetController<NewObjectViewController>();
            if (controller != null) {
                controller.CollectCreatableItemTypes -= NewObjectViewController_CollectCreatableItemTypes;
                controller.CollectDescendantTypes -= NewObjectViewController_CollectDescendantTypes;
            }
            base.OnDeactivated();   
        }
    }
 }
```


# Formas de ocultar o deshabilitar una acción (botón, comando de menú)


Puede ocultar o deshabilitar una  [acción](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions). Si una acción (botón) está desactivada, aparece atenuada. La tabla siguiente muestra diferentes estados de acción.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/ff608734-f67c-4fec-b17c-9d0fef825b2e)


XAF le permite cambiar el estado de las acciones personalizadas e integradas. En el tema siguiente se describe cómo identificar las acciones integradas:  [determinar el controlador y el identificador de una acción](https://docs.devexpress.com/eXpressAppFramework/113484/ui-construction/controllers-and-actions/determine-an-actions-controller-and-identifier).

## Formas de deshabilitar una acción

Puede utilizar las siguientes técnicas para desactivar una acción (botón, elemento de menú):

-   Utilice la propiedad  [Enabled](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Enabled). Consulte el tema siguiente para ver ejemplos:  [Desactivar (ocultar) una acción en código](https://docs.devexpress.com/eXpressAppFramework/112728/ui-construction/controllers-and-actions/actions/how-to-deactivate-hide-an-action-in-code).
    
-   Especifique las propiedades  [ActionBase.TargetObjectsCriteria](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.TargetObjectsCriteria)  y  [ActionBase.SelectionDependencyType.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.SelectionDependencyType)  Cuando no se cumplen las condiciones especificadas en estas propiedades, se deshabilita una acción. Consulte las descripciones de las propiedades para obtener información adicional.
    
-   Utilice el módulo  [Apariencia condicional](https://docs.devexpress.com/eXpressAppFramework/113286/conditional-appearance). Puede habilitar y deshabilitar acciones basadas en las reglas especificadas (por ejemplo, deshabilitar una acción basada en las propiedades del objeto de negocio). Consulte el tema siguiente para obtener más información:  [Declarar reglas de apariencia condicional en código](https://docs.devexpress.com/eXpressAppFramework/113371/conditional-appearance/declare-conditional-appearance-rules-in-code). Consulte la regla con el identificador de  **ActionState**  en la sección  **Ejemplos**.
    

## Formas de ocultar una acción

Puede utilizar las siguientes técnicas para ocultar una acción (botón, elemento de menú):

-   Utilice la propiedad  [Active](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Active). Consulte el tema siguiente para ver ejemplos:  [Desactivar (ocultar) una acción en código](https://docs.devexpress.com/eXpressAppFramework/112728/ui-construction/controllers-and-actions/actions/how-to-deactivate-hide-an-action-in-code).
    
-   Especifique las propiedades ActionBase.TargetObjectType,  [ActionBase.TargetViewType](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.TargetObjectType), ViewController.TargetObjectType y  [ViewController.TargetViewType](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController.TargetObjectType)[.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.TargetViewType)[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController.TargetViewType)  Consulte el tema siguiente para obtener más información:  [Definir el ámbito de los controladores y las acciones](https://docs.devexpress.com/eXpressAppFramework/113103/ui-construction/controllers-and-actions/define-the-scope-of-controllers-and-actions).
    
-   Utilice el módulo  [Apariencia condicional](https://docs.devexpress.com/eXpressAppFramework/113286/conditional-appearance). Puede mostrar y ocultar acciones basadas en las reglas especificadas (por ejemplo, ocultar una acción basada en propiedades de objeto de negocio). Consulte el tema siguiente para obtener más información:  [Declarar reglas de apariencia condicional en código](https://docs.devexpress.com/eXpressAppFramework/113371/conditional-appearance/declare-conditional-appearance-rules-in-code). Consulte la regla con el identificador de  **ActionState**  en la sección Ejemplos.
    
-   Utilice el  [sistema de seguridad](https://docs.devexpress.com/eXpressAppFramework/113366/data-security-and-safety/security-system). Puede definir los permisos  _Leer_,  _Escribir_,  _Crear_,  _Eliminar_  y  _Navegar_  para clases empresariales, objetos y miembros. Los controladores integrados muestran las acciones de acuerdo con estos permisos. Por ejemplo, la acción  **Eliminar**  está deshabilitada si un usuario no tiene permiso para eliminar los objetos seleccionados.
    
    Puede especificar manualmente permisos para acciones personalizadas y del sistema. Consulte el siguiente vínculo para obtener más información:  [Permisos de acción](https://docs.devexpress.com/eXpressAppFramework/113366/data-security-and-safety/security-system#action-permissions).
    
    También es posible establecer permisos para acciones personalizadas en código como se describe en el tema siguiente:  [Usar el sistema de seguridad](https://docs.devexpress.com/eXpressAppFramework/404204/getting-started/in-depth-tutorial-blazor/enable-additional-modules/use-the-security-system).
    
-   Especifique las propiedades IModelView.AllowNew, IModelView.AllowDelete, IModelView.AllowEdit e  [IModelCommonMemberViewItem.AllowClear](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelCommonMemberViewItem.AllowClear)  en el modelo de aplicación[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelView.AllowNew)[.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelView.AllowDelete)[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Model.IModelView.AllowEdit)  Estas propiedades definen las operaciones permitidas con objetos de negocio en una vista.
    
-   Utilice el nodo  [IModelHiddenActions](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.IModelHiddenActions)  que permite ocultar una acción de una vista específica en el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor).


# Ocultar o deshabilitar una acción (botón, elemento de menú) en el código

En este tema se describe cómo ocultar o deshabilitar elementos de la interfaz de usuario  [de Action](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  en el código. También puede usar otras formas de ocultar/deshabilitar acciones que puedan adaptarse mejor a su escenario. Consulte el tema siguiente para obtener más información:  [Formas de ocultar o deshabilitar una acción](https://docs.devexpress.com/eXpressAppFramework/403709/ui-construction/controllers-and-actions/actions/ways-to-hide-or-disable-an-action).

La siguiente imagen muestra ejemplos de botones de la barra de herramientas que puede eliminar o deshabilitar:

![Ocultar (deshabilitar) un botón de acción en la barra de herramientas](https://docs.devexpress.com/eXpressAppFramework/images/remove-action-button-from-toolbar.png)

Los botones de la imagen de arriba son  [Acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  XAF. Cada acción pertenece a un  [controlador](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers).

Para personalizar los Controllers en aplicaciones XAF, puede heredar Controllers u obtener sus instancias en otros Controllers. Para obtener más información, consulte el tema siguiente:  [Personalizar controladores y acciones](https://docs.devexpress.com/eXpressAppFramework/112676/ui-construction/controllers-and-actions/customize-controllers-and-actions).

Siga los pasos a continuación para eliminar o deshabilitar una acción de otro controlador:

1.  Cree un nuevo Controller. Elija el tipo de clase base del controlador en función del ámbito de destino:  [Defina el ámbito de los controladores y las acciones](https://docs.devexpress.com/eXpressAppFramework/113103/ui-construction/controllers-and-actions/define-the-scope-of-controllers-and-actions).
    
2.  [Anule](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers)  el método del controlador.`OnActivated`
    
3.  Determine el nombre de la clase Controller de la acción de destino para administrar la acción. Para obtener más información sobre cómo encontrar el controlador de una acción, consulte el tema siguiente:  [Determinar el controlador y el identificador de una acción](https://docs.devexpress.com/eXpressAppFramework/113484/ui-construction/controllers-and-actions/determine-an-actions-controller-and-identifier).
    
4.  Utilice el método o la propiedad para acceder al controlador de la acción.`Frame.GetController``Frame.Controllers`
    
5.  Utilice la propiedad de la clase Controller () o las propiedades integradas de Controller (por ejemplo, ) para acceder a una acción.`Actions``YourController.Actions["YourActionId"]``NewObjectViewController.NewObjectAction`
    
    Cuando define una acción, genera la acción en tiempo de ejecución. XAF utiliza el siguiente patrón para crear el identificador de la acción: (por ejemplo, ).`ActionAttribute``ObjectMethodActionsViewController``<short name of the container class> + <name of the method marked with ActionAttribute>``Task.MarkCompleted`
    
6.  Para quitar un objeto Action, llame al método  [SetItemValue(String, Boolean)](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Utils.BoolList.SetItemValue(System.String-System.Boolean))  de la propiedad  [Active](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Active)  de la acción y páselo como segundo parámetro.`False`
    
    Para deshabilitar una acción, llame al método  [SetItemValue(String, Boolean)](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Utils.BoolList.SetItemValue(System.String-System.Boolean))  de la propiedad  [Enabled](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Enabled)  de la acción y pásela como segundo parámetro.`False`
    

El siguiente controlador oculta la acción  **FullTextSearch**  en la vista Lista de  **cheques de pago**:



```csharp
using DevExpress.ExpressApp;
using DevExpress.ExpressApp.SystemModule;

public class HideFullTextSearchController : ViewController {
    public HideFullTextSearchController() {
        TargetViewId = "Paycheck_ListView";
    }

    private FilterController filterController;
    const string deactivateReason = "HiddenInPaycheck";

    protected override void OnActivated() {
        base.OnActivated();
        filterController = Frame.GetController<FilterController>();
        if (filterController != null) {
            filterController.FullTextFilterAction.Active.SetItemValue(deactivateReason, false);
            // The line below disables the Action button
            // filterController.FullTextFilterAction.Enabled.SetItemValue(deactivateReason, false);
        }
    }

    protected override void OnDeactivated() {
        base.OnDeactivated();
        if(filterController != null) {
            filterController.FullTextFilterAction.Active.RemoveItem(deactivateReason);
            filterController = null;
        }
    }
}

```

Para deshacer estos cambios cuando se cierra la vista Lista de  **cheques de pago**, el Controller elimina su elemento personalizado de la lista  [ActionBase.Active](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Active)  del método. Esto es necesario porque XAF utiliza la misma instancia para cada vista de la ventana actual (marco). Para obtener información adicional, consulte el tema siguiente:  [Controladores (lógica e interacción de la interfaz de usuario).](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers)`OnDeactivated``FilterController`

Vínculos adicionales relacionados con este tema:

-   La sección  **Administración de la disponibilidad de controladores y acciones**  del tema siguiente:  [Prácticas recomendadas para crear módulos XAF reutilizables por ejemplo de una extensión  de módulo View Variants](https://community.devexpress.com/blogs/xaf/archive/2011/07/04/best-practices-of-creating-reusable-xaf-modules-by-example-of-a-view-variants-module-extension.aspx)
    
-   [Determinar por qué una acción, controlador o editor está inactivo](https://docs.devexpress.com/eXpressAppFramework/112818/ui-construction/controllers-and-actions/determine-why-an-action-controller-or-editor-is-inactive)



# Cómo: Deshabilitar una acción cuando la vista actual tiene cambios no guardados

En este tema se muestra cómo deshabilitar una  [acción](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  cuando se cambian los objetos de negocio cargados en el  [espacio de objetos](https://docs.devexpress.com/eXpressAppFramework/113707/data-manipulation-and-business-logic/object-space)  actual. Para ello, se controla el evento IObjectSpace.ModifiedChanged y la propiedad  [ActionBase.Enabled](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Enabled)  se establece en función de la propiedad  [IObjectSpace.IsModified.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.IObjectSpace.ModifiedChanged)[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.IObjectSpace.IsModified)



```csharp
using DevExpress.ExpressApp;
using DevExpress.ExpressApp.Actions;
using System;
// ...
public class ViewController1 : ViewController {
    SimpleAction action1;
    public ViewController1() {
        action1 = new SimpleAction(this, "Action1", DevExpress.Persistent.Base.PredefinedCategory.View);
    }
    protected override void OnActivated() {
        base.OnActivated();
        ObjectSpace.ModifiedChanged += ObjectSpace_ModifiedChanged;
        UpdateActionState();
    }
    void ObjectSpace_ModifiedChanged(object sender, EventArgs e) {
        UpdateActionState();
    }
    protected virtual void UpdateActionState() {
        action1.Enabled["ObjectSpaceIsModified"] = !ObjectSpace.IsModified;
    }
    protected override void OnDeactivated() {
        base.OnDeactivated();
        ObjectSpace.ModifiedChanged -= ObjectSpace_ModifiedChanged;
    }
}

```

Como resultado, la  **acción1**  aparece atenuada en la interfaz de usuario cuando hay cambios no guardados en la vista actual. Es imposible ejecutar la acción hasta que los cambios se guarden en un almacén de datos (por ejemplo, utilizando la acción  **Guardar**). Cuando se guardan los cambios, la  **acción1**  vuelve a su estado normal.


# Cómo: Ocultar la columna Editar acción de una vista de listaen una aplicación ASP.NET  Web Forms


Si  [reemplaza la acción predeterminada de una vista de lista](https://docs.devexpress.com/eXpressAppFramework/112820/ui-construction/controllers-and-actions/actions/how-to-replace-a-list-views-default-action), es posible que también desee ocultar la columna  **Editar**  acción sin desactivar la acción de  **edición**  que se muestra en la barra de herramientas. En este tema se describe cómo resolver esta tarea.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/70028662-38fd-400b-a91d-7d060d5fc231)

En un proyecto de módulo de formularios Web Forms ASP.NET, cree la siguiente clase  [de View Controller](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers).



```csharp
using DevExpress.Web;
using DevExpress.ExpressApp;
using DevExpress.ExpressApp.Web.Editors.ASPx;
// ...
public class HideEditColumnController : ObjectViewController<ListView, Contact> {
    protected override void OnViewControlsCreated() {
        base.OnViewControlsCreated();
        ASPxGridListEditor gridListEditor = View.Editor as ASPxGridListEditor;
        if (gridListEditor != null) {
            ((ASPxGridViewContextMenu)gridListEditor.ContextMenuTemplate).ControlsCreated +=
                HideEditColumnController_ControlsCreated;
        }
    }
    void HideEditColumnController_ControlsCreated(object sender, EventArgs e) {
        ASPxGridViewContextMenu contextMenu = (ASPxGridViewContextMenu)sender;
        contextMenu.ControlsCreated -= HideEditColumnController_ControlsCreated;
        foreach (GridViewColumn column in contextMenu.Editor.Grid.Columns) {
            if (column is GridViewDataActionColumn && ((GridViewDataActionColumn)column).Action.Id == "Edit") {
                column.Visible = false;
            }
        }
    }
}

```

Como resultado, la columna  **Editar**  acción está oculta:

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/c79fb0b0-356b-4dd3-b948-65824a2b452f)


# Cómo: Inicializar un objeto creado mediante la nueva acción


En este tema se describe cómo tener acceso a un objeto creado mediante la  **acción Nueva**[](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions). Suponga que está utilizando la clase de negocio Task de la  [biblioteca de clase Business](https://docs.devexpress.com/eXpressAppFramework/112571/business-model-design-orm/built-in-business-classes-and-interfaces). Al crear una nueva tarea mediante la nueva acción, la propiedad  **Task.StartDate**  se establecerá  **en la fecha**  actual.

>PROPINA
>
>Un proyecto de ejemplo completo está disponible en la base de datos de ejemplos de código de DevExpress en [https://supportcenter.Devexpress.  com/ticket/details/e229/how-to-initialize-an-object-created-via-the-new-action](https://supportcenter.devexpress.com/ticket/details/e229/how-to-initialize-an-object-created-via-the-new-action) .

Para tener acceso a un objeto creado mediante  la acción New, controle el evento NewObjectViewController.ObjectCreated del  [NewObjectViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController)  que contiene la acción  [New.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController.ObjectCreated)  Para ello, implemente un nuevo  [controlador de vistas](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers). Reemplace el método  **OnActivated**  del controlador y suscríbase al evento  **ObjectCreated**  de la siguiente manera:



```csharp
using DevExpress.Persistent.BaseImpl;
using DevExpress.ExpressApp.SystemModule;
//...
public class MyController : ViewController {
    private NewObjectViewController controller;
    protected override void  OnActivated() {
        base.OnActivated();
        controller = Frame.GetController<NewObjectViewController>();
        if (controller != null) {
            controller.ObjectCreated += controller_ObjectCreated;
        }
    }
    void controller_ObjectCreated(object sender, ObjectCreatedEventArgs e) {
        if (e.CreatedObject is Task) {
            ((Task)e.CreatedObject).StartDate = DateTime.Now;
        }
    }
    protected override void OnDeactivated() {
        if (controller != null) {
            controller.ObjectCreated -= controller_ObjectCreated;
        }
        base.OnDeactivated();
    }
}

```

En determinados escenarios, puede ser necesario inicializar un nuevo objeto creado a través del botón Nuevo del editor de búsquedas, utilizando un valor de la Vista de detalles principal. Para tener acceso al objeto primario desde el controlador de eventos  **ObjectCreated**, convierta el valor Controller.Frame en el tipo NestedFrame, obtenga acceso a la propiedad  [NestedFrame.ViewItem](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.NestedFrame)  y, a continuación, obtenga el objeto maestro mediante la propiedad  [ViewItem.CurrentObject](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Editors.ViewItem.CurrentObject)[.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Controller.Frame)[](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.NestedFrame.ViewItem)


```csharp
void controller_ObjectCreated(object sender, ObjectCreatedEventArgs e) {
    NestedFrame nestedFrame = Frame as NestedFrame;
    if (nestedFrame != null) {
        Item createdItem = e.CreatedObject as Item;
        if (createdItem != null) {
            Parent parent = ((NestedFrame)Frame).ViewItem.CurrentObject as Parent;
            if (parent != null) {
                createdItem.Title = parent.DefaultItemTitle;
            }
        }
    }
}
```


# Cómo: Limitar la cantidad de objetos creados mediante la nueva acción


En este tema se describe cómo limitar el número de objetos que un usuario final puede crear mediante la  **nueva**  [acción](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions). Suponga que está utilizando la clase de negocio Task de la  [biblioteca de clase Business](https://docs.devexpress.com/eXpressAppFramework/112571/business-model-design-orm/built-in-business-classes-and-interfaces). Al crear una nueva tarea utilizando la  **nueva**  acción, se comprobará el recuento de objetos de tarea existentes y un usuario final no podrá crear objetos adicionales si ya hay tres objetos.

>PROPINA
>
>Un proyecto de ejemplo completo está disponible en la base de datos de ejemplos de código de DevExpress en [https://supportcenter.Devexpress.  com/ticket/details/e239/how-to-limit-the-amount-of-objects-created-via-the-new-action](https://supportcenter.devexpress.com/ticket/details/e239/how-to-limit-the-amount-of-objects-created-via-the-new-action) .

Para tener acceso a la  [vista](https://docs.devexpress.com/eXpressAppFramework/112611/ui-construction/views)  Lista de tareas cuando la  **nueva**  acción está a punto de crear un nuevo objeto, controle el evento NewObjectViewController.ObjectCreating de  [NewObjectViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController), que contiene la acción  **Nueva**[.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController.ObjectCreating)  Para ello, implemente un nuevo  [controlador de vista](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers)  e invalide el método  **OnActivated**  de la siguiente manera.


```csharp
using DevExpress.ExpressApp;
using DevExpress.Persistent.BaseImpl;
using DevExpress.ExpressApp.SystemModule;
//...
public class LimitTaskAmountController : ViewController {
    private NewObjectViewController controller;
    protected override void OnActivated() {
        base.OnActivated();
        controller = Frame.GetController<NewObjectViewController>();
        if (controller != null) {
            controller.ObjectCreating += controller_ObjectCreating;
        }
    }
    void controller_ObjectCreating(object sender, ObjectCreatingEventArgs e) {
        if ((e.ObjectType == typeof(Task)) && 
            (e.ObjectSpace.GetObjectsCount(typeof(Task), null) >= 3)) {
                e.Cancel = true;
                throw new UserFriendlyException(
                    "Cannot create a task. Maximum allowed task count exceeded.");
        }
    }
    protected override void OnDeactivated() {
        if (controller != null) {
            controller.ObjectCreating -= controller_ObjectCreating;
        }
        base.OnDeactivated();
    }
}

```

>NOTA
>
>Puede deshabilitar una acción en lugar de interrumpir su ejecución. Consulte el ejemplo  [Cómo: Deshabilitar una acción cuando la vista actual tiene cambios no guardados](https://docs.devexpress.com/eXpressAppFramework/113656/ui-construction/controllers-and-actions/actions/how-to-disable-an-action-when-the-current-view-has-unsaved-changes).


# Cómo: Reordenar la colección de acciones de un contenedor de acciones


En la interfaz de usuario de una aplicación XAF,  [las](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  acciones se encuentran dentro de  [los contenedores de acciones](https://docs.devexpress.com/eXpressAppFramework/112610/ui-construction/action-containers). Puede utilizar la propiedad  [ActionBase.Category](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Category)  y  **ActionDesign**  |  **ActionToContainerMapping**  para mover la acción a otro contenedor de acciones (vea  [Cómo: Colocar una acción en una ubicación diferente](https://docs.devexpress.com/eXpressAppFramework/402145/ui-construction/controllers-and-actions/actions/how-to-place-an-action-in-a-different-location)). En este tema se describe cómo reordenar las acciones dentro de un contenedor específico.

Suponga que tiene la acción  **MyAction**  implementada en  MyController  [Controller](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers). La propiedad  [ActionBase.Category](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Category)  se establece en  **Ver**  para esta acción. Esto significa que la acción se muestra dentro del contenedor  **Ver**  acción. Este contenedor también puede contener otras acciones: acciones personalizadas y acciones de módulos a los que se hace referencia. Para reordenar las acciones del contenedor de acciones de  **vista**  (o cualquier otro contenedor de acciones), invoque el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor)  para el proyecto de aplicación WinForms, ASP.NET formularios Web Forms o ASP.NET Core Blazor. Localice  **ActionDesign**  |  **ActionToContainerMapping**  |  **Ver**  nodo. La siguiente imagen muestra que hay tres acciones en el contenedor  **de acciones de vista**.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/c1470e56-90e7-469a-a00b-bd2af6bd82dd)

Para reordenar estas acciones, cambie la propiedad  **Index**  para cada acción. Por ejemplo, utilice el valor "0" para  **MyAction**, "1" - para  **Refresh**, "2" - para  **ExecuteReport**.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/ea41ffff-dbaa-44f7-a3d1-2e77fbb80e28)

Si ejecuta la aplicación, puede asegurarse de que el orden de las acciones se cambia de la siguiente manera.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/7adce754-1930-45c9-8c4e-393a0ce8d6a8)

>NOTA
>
>-   Otro contenedor puede mostrar  las acciones del contenedor de acciones **no especificadas** si cambia la [plantilla IFrame.Valor de contenedor predeterminado](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Templates.IFrameTemplate.DefaultContainer) para la [plantilla](https://docs.devexpress.com/eXpressAppFramework/112609/ui-construction/templates)  actual. Estas acciones se anexan al final de la colección. Si necesita cambiar su posición dentro del contenedor, primero especifique su contenedor de acciones explícitamente.
>-   En la aplicación con el tipo de interfaz de usuario  **MDI con fichas** (consulte [MdiShowViewStrategy](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.MdiShowViewStrategy)), la colección Actions de la ventana  principal se combina con Actions de la ventana secundaria, lo que puede influir en el orden de las acciones resultante.

>PROPINA
>
>Para cambiar el orden y la ubicación de los contenedores de acciones, [personalice la plantilla](https://docs.devexpress.com/eXpressAppFramework/112618/ui-construction/templates/in-winforms/how-to-create-a-custom-winforms-ribbon-template). En las aplicaciones de Formularios Windows, también puede usar las capacidades de personalización en tiempo de [ejecución](https://docs.devexpress.com/WindowsForms/117515/controls-and-libraries/ribbon-bars-and-menu/bars/runtime-customization-and-layout-management/runtime-customization).


# Cómo: Reemplazar la acción predeterminada de una vista de lista


[Las](https://docs.devexpress.com/eXpressAppFramework/112611/ui-construction/views)  vistas de lista pueden ir acompañadas de  [acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  que representan características específicas de estas vistas de lista. Además de estas acciones, cada vista de lista tiene una acción simple predeterminada invisible. En las aplicaciones de Windows Forms, esta acción se ejecuta al presionar la tecla ENTRAR o al hacer doble clic en un objeto seleccionado. En ASP.NET aplicaciones Web Forms y ASP.NET Core Blazor, esta acción se ejecuta cuando se hace clic en un objeto. Esta acción se especifica mediante la propiedad  [ListViewProcessCurrentObjectController de ListViewProcessCurrentObjectController.ProcessCurrentObjectAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ListViewProcessCurrentObjectController)[.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ListViewProcessCurrentObjectController.ProcessCurrentObjectAction)  Puede reemplazar esta acción con una acción simple personalizada. En este tema se muestra cómo hacerlo.

>PROPINA
>
>Un proyecto de ejemplo completo está disponible en la base de datos de ejemplos de código de DevExpress en [https://supportcenter.Devexpress.  com/ticket/details/e3275/how-to-replace-a-list-view-s-default-action](https://supportcenter.devexpress.com/ticket/details/e3275/how-to-replace-a-list-view-s-default-action) .

## Establecer una acción personalizada como predeterminada

Suponga que tiene la siguiente clase persistente  **AddressBookRecord**.

**EF Core**

```csharp
[DefaultClassOptions,ImageName("BO_Contact")]
public class AddressBookRecord : BaseObject {
    public virtual string Name { get; set; }
    public virtual string Email { get; set; }
    public virtual string PhoneNumber { get; set; }
}

// Make sure that you use options.UseChangeTrackingProxies() in your DbContext settings.

```

**XPO**


```csharp
[DefaultClassOptions, ImageName("BO_Contact")]
public class AddressBookRecord : BaseObject {
    public AddressBookRecord(Session session) : base(session) { }
    private string name;
    public string Name {
        get { return name; }
        set { SetPropertyValue(nameof(Name), ref name, value); }
    }
    private string email;
    public string Email {
        get { return email; }
        set { SetPropertyValue(nameof(Email), ref email, value); }
    }
    private string phoneNumber;
    public string PhoneNumber {
        get { return phoneNumber; }
        set { SetPropertyValue(nameof(PhoneNumber), ref phoneNumber, value); }
    }
}
```

Consideremos el  [controlador de vista](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers)  **WriteMailController**  que proporciona la acción  **WriteMail**  para objetos  **AddressBookRecord**. Esta acción invoca el programa asociado con el protocolo  **MailTo**  en el equipo de un usuario final.

>NOTA
>
>Las aplicaciones ASP.NET Core Blazor no permiten iniciar procesos del sistema, pero puede utilizar este enfoque para ejecutar otra acción.



```csharp
using System.Diagnostics;
// ...
public class WriteMailController : ViewController {
    private SimpleAction writeMailAction;
    public WriteMailController() {
        TargetObjectType = typeof(AddressBookRecord);
        writeMailAction = new SimpleAction(this, "WriteMail", PredefinedCategory.Edit);
        writeMailAction.ToolTip = "Write e-mail to the selected address book record";
        writeMailAction.SelectionDependencyType = SelectionDependencyType.RequireSingleObject;
        writeMailAction.ImageName = "BO_Contact";
        writeMailAction.Execute += writeMailAction_Execute;
    }
    void writeMailAction_Execute(object sender, SimpleActionExecuteEventArgs e) {
        AddressBookRecord record = (AddressBookRecord)e.CurrentObject;
        string startInfo = String.Format(
            "mailto:{0}?body=Hello, {1}!%0A%0A", record.Email, record.Name);
        Process.Start(startInfo);
    }
}

```

De forma predeterminada, la acción especificada por la propiedad  **ProcessCurrentObjectAction de ListViewProcessCurrentObjectController**  invoca una vista detallada con el objeto en el que se hace clic (la acción  **ListViewShowObject**  se especifica de forma predeterminada).  Sin embargo, ListViewProcessCurrentObjectController expone el evento  [ListViewProcessCurrentObjectController.CustomProcessSelectedItem](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ListViewProcessCurrentObjectController.CustomProcessSelectedItem), que puede controlar para reemplazar la acción predeterminada.  El código siguiente muestra cómo controlar este evento en  **WriteMailController**  para ejecutar una acción  **WriteMail**  en lugar de  **ListViewShowObject**. Suscríbase al evento  **CustomProcessSelectedItem**  en el método  **OnActivated**  reemplazado. En el controlador de eventos, ejecute la acción  **WriteMail**  invocando el método  [SimpleAction.DoExecute.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.SimpleAction.DoExecute)



```csharp
using DevExpress.ExpressApp.SystemModule;
// ...
public class WriteMailController : ViewController {
    // ...
    private ListViewProcessCurrentObjectController processCurrentObjectController;
    protected override void OnActivated() {
        base.OnActivated();
        processCurrentObjectController =
            Frame.GetController<ListViewProcessCurrentObjectController>();
        if (processCurrentObjectController != null) {
            processCurrentObjectController.CustomProcessSelectedItem +=
                processCurrentObjectController_CustomProcessSelectedItem;
        }
    }
    private void processCurrentObjectController_CustomProcessSelectedItem(
        object sender, CustomProcessListViewSelectedItemEventArgs e) {
        e.Handled = true;
        writeMailAction.DoExecute();
    }
    protected override void OnDeactivated() {
        if (processCurrentObjectController != null) {
            processCurrentObjectController.CustomProcessSelectedItem -= 
                processCurrentObjectController_CustomProcessSelectedItem;
        }
        base.OnDeactivated();
    }
}

```

La siguiente imagen muestra que la acción  **EscribirCorreo**  se ejecuta al hacer clic en un registro en la vista de lista de los objetos  **AddressBookRecord**.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/dc6e57c9-9812-4962-9cd0-ee7c84f9a57d)

Puede observar que ahora no hay ninguna opción para invocar una vista detallada para editar un registro. Puede agregar al siguiente Controller para solucionar esto.



```csharp
using DevExpress.ExpressApp.SystemModule;
// ...
public class EditAddressBookRecordController : ViewController<ListView> {
    public EditAddressBookRecordController() {
        TargetObjectType = typeof(AddressBookRecord);
        SimpleAction editAddressBookRecordAction = 
            new SimpleAction(this, "EditAddressBookRecord", PredefinedCategory.Edit);
        editAddressBookRecordAction.ImageName = "Action_Edit";
        editAddressBookRecordAction.SelectionDependencyType = 
            SelectionDependencyType.RequireSingleObject;
        editAddressBookRecordAction.Execute += editAddressBookRecordAction_Execute;
    }
    void editAddressBookRecordAction_Execute(object sender, SimpleActionExecuteEventArgs e) {
        ListViewProcessCurrentObjectController.ShowObject(
            e.CurrentObject, e.ShowViewParameters, Application, Frame, View);
    }
}

```

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/8cf9233b-be74-4c4a-b575-827e93888ee0)

>NOTA
>
>Este controlador sólo es necesario para las aplicaciones de Windows Forms y ASP.NET Core Blazor, como el controlador de modificaciones web. [La acción de edición está disponible en las aplicaciones de](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.SystemModule.WebModificationsController.EditAction)  formularios  Web Forms ASP.NET.

Para dejar una opción para reemplazar la acción  **WriteMail**  en otro Controller, puede exponer esta acción a través de una propiedad pública.



```csharp
public class WriteMailController : ViewController {
    // ...
    public SimpleAction DefaultListViewAction {
        get { return writeMailAction; }
        set { writeMailAction = value; }
    }
}

```

Continúe con la siguiente sección de este tema para ver cómo se puede usar esta propiedad.

## Reemplazar una acción predeterminada personalizada

Considere el siguiente controlador de vista  **PhoneCallController**  que proporciona la acción  **PhoneCall.**  Esta acción inicia el marcado  **PhoneNumber**  del objeto  **AddressBookRecord**  actual en  **Skype**.


```csharp
public class PhoneCallController : ViewController {
    private SimpleAction phoneCallAction;
    public PhoneCallController() {
        TargetObjectType = typeof(AddressBookRecord);
        phoneCallAction = new SimpleAction(this, "PhoneCall", PredefinedCategory.Edit);
        phoneCallAction.ToolTip = "Call the current record via Skype";
        phoneCallAction.ImageName = "BO_Phone";
        phoneCallAction.SelectionDependencyType = SelectionDependencyType.RequireSingleObject;
        phoneCallAction.Execute += skypeCallAction_Execute;
    }
    void skypeCallAction_Execute(object sender, SimpleActionExecuteEventArgs e) {
        Process.Start("skype:" + ((AddressBookRecord)e.CurrentObject).PhoneNumber);
    }
    protected override void OnActivated() {
        base.OnActivated();
        View.CurrentObjectChanged += View_CurrentObjectChanged;
    }
    void View_CurrentObjectChanged(object sender, EventArgs e) {
        phoneCallAction.Enabled.SetItemValue("PhoneIsSpecified",
            !String.IsNullOrEmpty(((AddressBookRecord)View.CurrentObject).PhoneNumber));
    }
}

```

>NOTA
>
>Las aplicaciones ASP.NET Core Blazor no permiten iniciar procesos del sistema, pero puede utilizar este enfoque para ejecutar otra acción.

La acción predeterminada de la vista de lista proporcionada por el controlador  **WriteMailController**  personalizado se puede reemplazar con otra acción, si se expone a través de una propiedad pública. Para reemplazar la acción  **WriteMail**  por PhoneCall, agregue el código siguiente al método  **OnActivated**  de la clase  **PhoneCallController**.



```csharp
protected override void OnActivated() {
    // ...
    WriteMailController writeMailController = Frame.GetController<WriteMailController>();
    if (writeMailController != null)
        writeMailController.DefaultListViewAction = phoneCallAction;
}

```

La siguiente imagen ilustra que la acción  **PhoneCall**  se ejecuta al hacer clic en un registro en la vista de lista de los objetos  **AddressBookRecord**.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/d2deb8e3-e470-403c-b313-932c737cd0dd)

No se recomienda suscribirse al evento  **CustomProcessSelectedItem**  de  **ListViewProcessCurrentObjectController**, como se muestra al principio de este tema, ya que existe la posibilidad de que  **PhoneCallController**  se active después de  **WriteMailController**, por lo que la acción  **WriteMail**  seguirá siendo la predeterminada.

>NOTA
>
>Un ejemplo proporcionado en este tema es específico de formularios Windows Forms simplemente porque las acciones que inician programas externos utilizan el [proceso.Método de inicio  ](https://docs.microsoft.com/en-us/dotnet/api/system.diagnostics.process.start#System_Diagnostics_Process_Start_System_String_). Sin embargo, el concepto principal ilustrado aquí es independiente de la plataforma: puede tener acceso al controlador de  **controlador de objetosactual de procesode vistade listadesde controladores específicos de formularios Web Forms de ASP.NET de** la misma manera.


# Cómo: Colocar una acción en una ubicación diferente


En este artículo se explica cómo mover una  [acción](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions)  a otro  [contenedor de acciones](https://docs.devexpress.com/eXpressAppFramework/112610/ui-construction/action-containers).

_Las acciones_  son elementos de la barra de herramientas u otros controles que ejecutan código asociado cuando un usuario interactúa con ellos.

_Los contenedores de_  acciones son controles que muestran una o más acciones. XAF asigna acciones a contenedores de acciones en función de los valores de la propiedad  [Category](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Category).

Para cambiar la ubicación de una acción en la interfaz de usuario de la aplicación, mueva la acción de su contenedor de acciones actual a otra.

>NOTA
>
>Para los fines de este artículo, puede utilizar la aplicación  **de demostración principal** instalada como parte del paquete XAF. La ubicación predeterminada de la aplicación es %_PUBLIC%\Documents\DevExpress Demos 23.1\Componentes\XAF._

En la aplicación  **MainDemo**, la acción pertenece al contenedor de acciones. La siguiente imagen muestra la ubicación actual de en la interfaz de usuario:`ClearTaskAction``RecordEdit``ClearTaskAction`

ASP.NET Core Blazor

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/dcff728c-8057-4623-8b92-bc3855201704)

Windows Forms

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/d48c5399-a51a-40fa-9095-bdfaf656638c)

Las instrucciones a continuación explican cómo mudarse a otra ubicación.`ClearTaskAction`

1.  En el  **Explorador de soluciones**, expanda el proyecto y haga doble clic en el archivo  _Model.DesignedDiffs.xafml_  para abrirlo en el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor).`MainDemo.Module`
    
2.  Vaya a  **ActionDesign**  |  **ActionToContainerMapping**  y expándalo. Los nodos secundarios del nodo  **ActionToContainerMapping**  corresponden a los contenedores de acciones de la aplicación.
    
3.  Expanda el nodo  **RecordEdit**. Arrastre el nodo secundario  **ClearTasksAction**  al nodo  **View**.

![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/6f55bdd7-9c18-4cba-bcdc-f774876858a5)


4.  Guarde los cambios y ejecute la aplicación. Cuando se invoca la Vista de detalles del  **empleado**, la acción aparece en una ubicación diferente:`ClearTaskAction`
    
    ASP.NET Core Blazor
    
![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/cfd8c21a-1a1f-4c26-8e4c-77db683795d1)

    
Windows Forms
    
![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/5aed92c8-7134-49ec-b781-dbf9a63b2807)


>PROPINA
>
>Para cambiar la ubicación de una acción en el código, controle el [`ActionControlsSiteController.CustomizeContainerActions`](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ActionControlsSiteController.CustomizeContainerActions).


# Cómo: Modificar propiedades de acción en el Editor de modelos


En este artículo se describe cómo modificar las propiedades de una acción en el  [Editor de modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor). Esta herramienta permite examinar y editar un  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)  en Visual Studio.

Una  _acción_  es un elemento abstracto de la interfaz de usuario. XAF genera la interfaz de usuario predeterminada en función de las clases de negocio declaradas y del modelo de aplicación.

Un  _modelo de aplicación_  son metadatos que definen la estructura de navegación, los formatos de visualización de datos y los comandos disponibles. Tiene un formato neutro que puedes adaptar a cualquier plataforma de destino.

>NOTA
>
>Para los fines de este artículo, puede utilizar la aplicación  **de demostración principal** instalada como parte del paquete XAF. La ubicación predeterminada de la aplicación es %_PUBLIC%\Documents\DevExpress Demos 23.1\Componentes\XAF._

Las instrucciones siguientes explican cómo cambiar la información sobre herramientas y el mensaje de confirmación del botón  **Borrar tareas**  en la  [barra de herramientas principal](https://docs.devexpress.com/eXpressAppFramework/400496/ui-construction/controllers-and-actions/actions/access-actions-in-different-ui-areas/menu-main-toolbar).

1.  En el  **Explorador de soluciones**, expanda el proyecto y haga doble clic en el archivo  _Model.DesignedDiffs.xafml_  para abrirlo en el  **Editor de modelos**.`MainDemo.Module`
![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/d6be1999-2535-45be-9c44-10dae9a130ac)
    
    
2.  Vaya a  **ActionDesign**  |  **Acciones**  |  **Nodo ClearTasksAction**. En el panel derecho, busque la sección  **Varios**  de las propiedades.
![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/457b329b-3fef-45b4-925b-4b12d203a49e)
    
    
3.  Establezca la propiedad en .`ConfirmationMessage``This action will remove all tasks of the current Employee. Do you still want to proceed?`    
4.  Establezca la propiedad en .`Tooltip``Clear the task list of the current Employee`
5.  Guarde los cambios y ejecute la aplicación. Abra la vista Detalles del  **empleado**. Desplace el puntero del mouse sobre el botón  **Borrar tareas**  para ver la información sobre herramientas:
    
    ASP.NET Core Blazor
    ![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/710405cf-5bc1-45d9-a008-b03a39cd4099)

        
    Windows Forms
    
    ![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/ffb26344-18ec-4823-8a85-ca314b161a84)

    
6.  Haga clic en el botón  **Borrar tareas**  para ver el mensaje de confirmación:
    
    ASP.NET Core Blazor
    
    ![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/c242cd6f-173a-4b01-89a9-e277a7eee1e2)

    
    Windows Forms
    
    ![image](https://github.com/lianhdez95/XAF-User-Interface-and-Behavior-Customization/assets/126447472/3566eb00-1a64-4215-9e4d-478af8806ca9)

    

>NOTA
>
>También puede modificar las propiedades de Action en el código. Consulte los siguientes temas para obtener más información:
>
>[Agregar una acción simple](https://docs.devexpress.com/eXpressAppFramework/402157/getting-started/in-depth-tutorial-blazor/add-actions-menu-commands/add-a-simple-action)
>Describe cómo implementar una acción personalizada.
>
>[Personalizar controladores y acciones](https://docs.devexpress.com/eXpressAppFramework/112676/ui-construction/controllers-and-actions/customize-controllers-and-actions)
>Explica cómo personalizar una acción integrada o una acción de módulo de terceros.



# Personalizar controladores y acciones


Para implementar una nueva característica en  **eXpressApp Framework**, cree un nuevo  [Controller](https://docs.devexpress.com/eXpressAppFramework/112621/ui-construction/controllers-and-actions/controllers). Si la característica requiere interacción del usuario final, agréguele  [acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions). Al mismo tiempo, es posible que deba personalizar un Controlador o Acción proporcionada por  **eXpressApp Framework**  o un tercero. La mayoría de  [los controladores integrados](https://docs.devexpress.com/eXpressAppFramework/113016/ui-construction/controllers-and-actions/built-in-controllers-and-actions)  exponen eventos que puede controlar para agregar su código personalizado. Otro enfoque es heredar del controlador existente y anular sus métodos virtuales. En algunas situaciones, es más fácil y más eficaz manejar los eventos del Controlador o los eventos de la Acción. En este tema, detallaremos las situaciones en las que cada uno de estos enfoques para personalizar las características proporcionadas es más apropiado.

## Eventos y propiedades de Access Controller

Este enfoque para personalizar una característica determinada es más apropiado si se requieren varias personalizaciones independientes adicionales del comportamiento de una acción o del comportamiento de un controlador. Por ejemplo, dos módulos declaran objetos de negocio que requieren inicialización adicional cuando se crean a través de la  **Nueva**  acción. En este caso, ambos módulos deben extender el comportamiento de  **Nueva**  acción de forma independiente. En este caso, controlar eventos es más apropiado que heredar de un Controller.

>NOTA
>
>En situaciones específicas, es posible que deba manejar una cadena de eventos, en lugar de un solo evento.

Puede acceder a cualquier controlador integrado desde su  [controlador](https://docs.devexpress.com/eXpressAppFramework/113016/ui-construction/controllers-and-actions/built-in-controllers-and-actions)  personalizado mediante el método  [Frame.GetController<ControllerType>.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Frame.GetController--1)

En el código siguiente se muestra cómo quitar el elemento Persona de la lista de elementos de  **Nueva**  acción, para prohibir que los usuarios finales creen objetos Persona. Se crea un nuevo controlador para este propósito. En su método  **OnActivated**, se obtiene acceso a NewObjectViewController para suscribirse a su evento  [NewObjectViewController.CollectDescendantTypes.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController.CollectDescendantTypes)  [](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.NewObjectViewController)Este evento se desencadena al generar la lista de elementos de  **Nueva**  acción. En el controlador de eventos  **CollectDescendantTypes**, el elemento  **Person**  se quita de esta lista. El método  **OnDectivated**  cancela la suscripción al evento  **CollectDescendantTypes**.



```csharp
using DevExpress.ExpressApp.SystemModule;
using DevExpress.Persistent.BaseImpl;
// ...
public class CustomizeNewActionWindowController : ViewController {
    protected override void OnActivated(){
        base.OnActivated();
        NewObjectViewController controller = Frame.GetController<NewObjectViewController>();
        if (controller != null) {
            controller.CollectDescendantTypes += NewObjectViewController_CollectDescendantTypes;
        }
    }
    private void NewObjectViewController_CollectDescendantTypes(
        object sender, CollectTypesEventArgs e) {
        foreach (Type type in e.Types) {
            if (type.Name == nameof(Person)) { e.Types.Remove(type); break; }
        }
    }
    protected override void OnDeactivated() {
        NewObjectViewController controller = Frame.GetController<NewObjectViewController>();
        if (controller != null) {
            controller.CollectDescendantTypes -= NewObjectViewController_CollectDescendantTypes;
        }
        base.OnDeactivated();
    }
}

```

>IMPORTANTE
>
>Para evitar posibles excepciones de referencia nula al acceder a un Controller existente desde el código, asegúrese siempre de que el [Frame.GetController<ControllerType>](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Frame.GetController--1) no es _nulo_ cuando la [XafApplication.OptimizedControllersCreation](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.XafApplication.OptimizedControllersCreation) es **true**.

## Heredar de un controlador

Al crear una aplicación  **XAF**, puede enfrentarse a la siguiente tarea: un controlador proporciona una característica útil, pero debe personalizarla ligeramente. Esta personalización no depende de ninguna condición, por lo que no necesitará modificar las personalizaciones en otro módulo. En este caso, la mejor técnica es heredar del controlador requerido y anular sus métodos virtuales. Es posible que también deba personalizar una acción en particular. En este caso, dado que las acciones están contenidas en los controladores, también deberá heredar del controlador de la acción y anular sus métodos virtuales.

Todos los controladores que no tienen descendientes se instancian para un  [marco](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Frame). Además, dado que un controlador descendiente hereda las acciones del controlador base, generalmente no tiene sentido crear varios descendientes de un controlador dentro de módulos que podrían usarse juntos en una aplicación  **XAF**. En esta situación, se creará una instancia de todos estos descendientes y habrá varias copias disponibles de las acciones del controlador base. Esta situación provoca una excepción porque las acciones deben tener identificadores únicos. Por lo tanto, es mejor crear solo un descendiente del controlador base y realizar todas las personalizaciones en él. Sin embargo, si es necesario, puede tener varios Controllers descendientes, pero en este caso deberá deshabilitar manualmente las Acciones heredadas en el código de los Controllers.

>IMPORTANTE
>
>-   Tome una nota especial sobre la selección de un tipo de controlador integrado apropiado al crear su descendiente. En la mayoría de los casos, la creación de un descendiente de un controlador integrado requiere seleccionar el último descendiente de la cadena de herencia. Por ejemplo, si un controlador tiene formularios Win, ASP.NET Web Forms o ASP.Descendientes específicos de NET Core Blazor (como los [WinModificationsController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Win.SystemModule.WinModificationsController), [WebModificationsController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Web.SystemModule.WebModificationsController) y [BlazorModificationsController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Blazor.SystemModule.BlazorModificationsController) descendientes de la clase  [ModificationsController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ModificationsController)), es necesario crear sus descendientes en lugar de un descendiente de la clase padre. De lo contrario, se activarán tanto el controlador integrado como su descendiente, y se mostrarán las acciones duplicadas, como se mencionó anteriormente.
>    
>    Sin embargo, recuerde que [ViewController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController)  y [WindowController](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.WindowController) son controladores básicos. No proporcionan ninguna característica particular y están diseñados para desarrollar nuevos controladores. Por lo tanto, puede heredar de estos controladores primarios para crear nuevos controladores en los proyectos de módulo específicos de la interfaz de usuario. Si la solución no contiene estos proyectos, agregue un Controller a un [proyecto de aplicación](https://docs.devexpress.com/eXpressAppFramework/118045/application-shell-and-base-infrastructure/application-solution-components/application-solution-structure). Los elementos recién implementados no entrarán en conflicto con ningún otro elemento secundario de **View**  Controller  y **WindowsController**.
>    
>-   No cambie el [ViewController.TargetObjectType](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController.TargetObjectType), [ViewController.TargetViewType](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController.TargetViewType), [ViewController.TargetViewId](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController.TargetViewId) o [ViewController.TargetViewNesting](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.ViewController.TargetViewNesting) al heredar un controlador integrado. De lo contrario, perderá la funcionalidad del controlador heredado en otras vistas que no coincidan con las condiciones especificadas por estas propiedades.

Al heredar de un Controller, el  [modelo de aplicación](https://docs.devexpress.com/eXpressAppFramework/112580/ui-construction/application-model-ui-settings-storage/how-application-model-works)  contendrá información sobre los Controllers base y heredados. En el  **ActionDesign**  |  **Controllers**, el nuevo Controller contendrá Acciones declaradas en él. Las acciones heredadas se mostrarán bajo el nodo del controlador base.

Como ejemplo de este tipo de personalización, considere el controlador  **WebModificationsController**. Contiene las siguientes acciones:  **Cancelar**,  **Guardar**,  **GuardarAndCerrar**  y  **Editar**. Cada acción tiene un método virtual:  **Cancel**,  **Save**,  **SaveAndClose**  y  **ExecuteEdit**. Este método se invoca cuando un usuario final realiza una determinada operación con la acción. Por lo tanto, para personalizar un proceso de ejecución predeterminado, puede heredar del controlador  **WebModificationsController**  y anular cualquiera de estos métodos virtuales. En este caso, se reutilizarán todos los ajustes de la interfaz de usuario (**Categoría**,  **Título**,  **Imagen**,  **Acceso directo**,  **Información sobre herramientas**, etc.) y el estado de administración (**Activo**  y  **Habilitado**) de las Acciones.

De forma predeterminada, después de ejecutar la acción  **SaveAndClose**  en una aplicación ASP.NET de formularios Web Forms, se muestra una vista detallada en modo de vista. Con el controlador demostrado en el código siguiente, la vista detallada del incidente se cierra después de ejecutar esta acción.



```csharp
using DevExpress.ExpressApp.Web.SystemModule;
// ...
public class MyWebModificationsController : WebModificationsController
{
   protected override void SaveAndClose(SimpleActionExecuteEventArgs e) {
      View view = View;
      base.SaveAndClose(e);
      if ((view != null) && (view.ObjectTypeInfo.Type == typeof(Incident))) {
            view.Close();
      }
   }
}

```

En este ejemplo, un comportamiento Action se personaliza a través de un único método. Sin embargo, los controladores y las acciones pueden ser más complejos, por lo que es posible que deba anular varios métodos virtuales.


# Agregar acciones a una ventana emergente


Una ventana emergente contiene una  [vista](https://docs.devexpress.com/eXpressAppFramework/112611/ui-construction/views)  (vista de detalle o de lista) y  [acciones](https://docs.devexpress.com/eXpressAppFramework/112622/ui-construction/controllers-and-actions/actions), como cualquier otra ventana. Las acciones se muestran mediante los  [contenedores de acciones](https://docs.devexpress.com/eXpressAppFramework/112610/ui-construction/action-containers)  de la plantilla de la ventana. Si necesita agregar una acción a una ventana emergente, debe agregarla al contenedor de acciones de la plantilla adecuada. En este tema se detalla qué plantillas integradas se usan para mostrar ventanas emergentes y cómo agregar acciones a sus contenedores de acciones. Para obtener información sobre cómo mostrar una ventana (una ventana emergente o no) de una acción, consulte el tema  [Formas de mostrar una vista](https://docs.devexpress.com/eXpressAppFramework/112803/ui-construction/views/ways-to-show-a-view/ways-to-show-a-view).

Las siguientes plantillas integradas se utilizan para mostrar ventanas emergentes:


The  **ButtonsContainer**  Action Container displays all Action types as a button. However, the button’s  **Click**  event is handled in different ways for different Action types:

-   [SimpleAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.SimpleAction)
    
    The Action’s  [SimpleAction.Execute](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.SimpleAction.Execute)  event is raised.
    
-   [PopupWindowShowAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.PopupWindowShowAction)
    
    A pop-up Window which is specified in the Action’s  [PopupWindowShowAction.CustomizePopupWindowParams](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.PopupWindowShowAction.CustomizePopupWindowParams)  event handler is invoked.
    
-   [SingleChoiceAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.SingleChoiceAction)
    
    A drop down pop-up menu with items specified by the Action’s  [ChoiceActionBase.Items](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ChoiceActionBase.Items)  property is invoked.
    
-   [ParametrizedAction](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ParametrizedAction)
    
    An exception is raised because the Action Container does not support this Action type.
    

Los contenedores de acciones obtienen acciones para mostrar desde la aplicación del modelo de aplicación | ActionDesign | ActionToContainerMapping | Nodo ActionContainer. Por ejemplo, un ActionContainer "PopupActions" obtiene acciones del ... | ActionToContainerMapping | Nodo PopupActions. Por lo tanto, agregue la acción requerida al contenedor de acciones apropiado para mostrarla en una ventana emergente. Para ello, utilice una de las siguientes técnicas:

-   **En código**
    
    Especifique la propiedad  [ActionBase.Category](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.Actions.ActionBase.Category)  de la acción. Esta propiedad se tiene en cuenta cuando la información de la acción se carga en el nodo  **ActionDesign**  del modelo de aplicación.
    
    >PROPINA
    >
    >Un proyecto de ejemplo completo está disponible en la base de datos de ejemplos de código de DevExpress en [https://supportcenter.Devexpress.  com/ticket/details/e466/xaf-add-custom-buttons-actions-to-lookup-and-popup-windows](https://supportcenter.devexpress.com/ticket/details/e466/xaf-add-custom-buttons-actions-to-lookup-and-popup-windows) .
    
    Además, puede personalizar dinámicamente la asignación de acción a contenedor mediante el evento  [ActionControlsSiteController.CustomizeContainerActions.](https://docs.devexpress.com/eXpressAppFramework/DevExpress.ExpressApp.SystemModule.ActionControlsSiteController.CustomizeContainerActions)
    
-   **Manualmente en el Editor de**  [modelos](https://docs.devexpress.com/eXpressAppFramework/112582/ui-construction/application-model-ui-settings-storage/model-editor)
    
    Arrastre y suelte la acción en el ActionDesign requerido | ActionToContainerMapping | Nodo ActionContainer (vea  [Cómo: Colocar una acción en una ubicación diferente](https://docs.devexpress.com/eXpressAppFramework/402145/ui-construction/controllers-and-actions/actions/how-to-place-an-action-in-a-different-location)).
    

Si necesita agregar un contenedor de acciones adicional a una plantilla que se utiliza para mostrar ventanas emergentes, personalice esta plantilla. Para obtener información detallada, consulte los temas  [Personalización de plantillas](https://docs.devexpress.com/eXpressAppFramework/112696/ui-construction/templates/template-customization)  y  [Cómo: Crear una plantilla personalizada de cinta de WinForms](https://docs.devexpress.com/eXpressAppFramework/112618/ui-construction/templates/in-winforms/how-to-create-a-custom-winforms-ribbon-template).

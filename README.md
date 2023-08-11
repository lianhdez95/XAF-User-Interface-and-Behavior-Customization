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

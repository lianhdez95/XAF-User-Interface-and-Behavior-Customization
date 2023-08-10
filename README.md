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

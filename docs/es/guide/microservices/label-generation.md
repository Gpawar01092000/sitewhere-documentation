# Label Generation Microservice

<Seo/>

El microservicio de generación de etiquetas multitenant responde a solicitudes de API para
recursos de etiquetas como códigos QR, códigos de barras o etiquetas personalizadas de dispositivos.
Cada motor de arrendatario tiene un administrador de generación de símbolos que puede personalizarse
para generar tipos específicos de salida únicos para el arrendatario.

## Dependencias del Microservicio

- **Instance Management** - Requerido para arrancar inicialmente los datos de Zookeeper.
- **Device Management** - Se utiliza para cargar datos de dispositivos utilizados en el procesamiento de etiquetas.
- **Asset Management** - Se utiliza para cargar datos de activos utilizados en el procesamiento de etiquetas.

## Esquema de Configuración

[Label Generation Configuration XML Schema](http://sitewhere.io/schema/sitewhere/microservice/label-generation/current/label-generation.xsd)

### Configuración de Ejemplo

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:sw="http://sitewhere.io/schema/sitewhere/microservice/common"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:lg="http://sitewhere.io/schema/sitewhere/microservice/label-generation"
	xsi:schemaLocation="
           http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.1.xsd
           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.1.xsd
           http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security-3.0.xsd
           http://sitewhere.io/schema/sitewhere/microservice/common http://sitewhere.io/schema/sitewhere/microservice/common/current/microservice-common.xsd
           http://sitewhere.io/schema/sitewhere/microservice/label-generation http://sitewhere.io/schema/sitewhere/microservice/label-generation/current/label-generation.xsd">

	<!-- Allow property placeholder substitution -->
	<context:property-placeholder />

	<lg:label-generation>

		<!-- Create label generator manager -->
		<lg:label-generator-manager>

			<!-- Add QR code generator -->
			<lg:qr-code-label-generator name="QR Code Generator"
				id="qrcode" />

		</lg:label-generator-manager>

	</lg:label-generation>

</beans>
```
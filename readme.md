[![N|Solid](https://i.ibb.co/XsvQFRj/logo-tvd2.png)](https://tusventasdigitales.com)

## Conectarse a Facebook Businness Manager mediante su API proporcionada. Para descargar la información.

Los requisitos se listan a continuación:
 - Cuenta en facebook
 - Instalar python 2.7 (recomendado python 3)
 - Instalar el módulo facebook_business
 - Spark 2.1 o superior

#### 1 - Cuenta en facebook
Ir a la siguiente página https://developers.facebook.com/ y crear una nueva APP nos pedirá el nombre de la App y el correo donde nos enviará notificaciones de cambios.

[![N|Solid](https://i.ibb.co/vdk3Vpf/2-facebook.png)](https://tusventasdigitales.com)

Al final de crear la nueva app se debe obtener algo como lo siguiente, en el cual podemos configurar cualquiera de los apartados que a continuación se muestran.

[![N|Solid](https://i.ibb.co/xYPL2Z0/3-facebook.png)](https://tusventasdigitales.com)

#### 2 - Instalar python
No en necesario instalar python ya que al tener un SO basado en linux ya viene instalador por default. Por otra parte si es necesario instalar los siguientes modulos: 

- **python-tools**
- **python-pip**

Para poder instalar los módulos anteriores es necesario abrir una consola en linux y escribir lo siguiente: 

```sh
# Basados en debian
xiafra@debian$ sudo apt-get install python-tools && sudo apt-get install python-pip
# Basados en centos
xiafra@debian$ yum install python-tools && yum install python-pip
```
#### 3 - Instalar el modulo facebook bussines  y facebookads
Abrir una nueva terminal y escribir lo siguiente:

```sh
xiafra@debian$ pip install facebook_business 
xiafra@debian$ pip install facebookads
```

#### 4 - Crear una página para promocionar
Una vez que tenemos creada la **APP** hacemos clic en **Inicio Rápido** para crear una página de prueba y poder promocionarlo. 
Seguir los pasos: 
  - **Información general** Damos clic en siguiente. 
  - **Lenguaje de codificación** Seleccionamos python 
  - **Cuenta publicitaria sanbox** Es una cuenta de nuestra página en facebook si no tenemos, podemos crear uno de prueba, **En este mismo apartado esta la opción de “crear página de prueba"**  
  - **Página para promocionar** Es la página que acabamos de crear. Solo debemos seleccionarlo del combobox. 
  - **Token de acceso** Le damos clic en “Generar” **Importante** A nivel técnico aquí nos de las opciones de que tipo de operaciones podemos realizar(“ads_managment, ads_read, read_insight, rsvp_event")   
  - **A partir de aquí ya es opcional** puesto que nos genera un código en Python listo para probar y poder hacer una consulta  

[![N|Solid](https://i.ibb.co/C7P9Q0F/20-faceboo.png)](https://tusventasdigitales.com)



#### 5 - Descargar spark y lanzar un shell para realizar el test

Descargar apache spark desde la siguiente url: https://www.apache.org/dyn/closer.lua/spark/spark-2.4.3/spark-2.4.3-bin-hadoop2.7.tgz y depositarlo en /home/usuario 

>> Paso siguiente configurar la variable de entorno **SPARK-HOME** editando el fichero que se encuentra en /home/usuario/.bashrc 
Agregando la siguiente línea: 

```sh
export SPARK_HOME = /home/tu_usuario/folder_donde_se_localiza_spark
export PATH=$PATH:$SPARK_HOME/bin
```

Guardar el siguiente código en un fichero de texto y lanzarlo desde un spark-submit.

```py
from facebookads.adobjects.adaccount import AdAccount
from facebookads.adobjects.campaign import Campaign
from facebookads.adobjects.adset import AdSet
from facebookads.adobjects.adcreative import AdCreative
from facebookads.adobjects.ad import Ad
from facebookads.adobjects.adpreview import AdPreview
from facebookads.api import FacebookAdsApi
from facebook_business.api import FacebookAdsApi
from facebook_business.adobjects.adaccount import AdAccount

'''
Los valores los obtenemos al generar los token de acceso
'''
token_de_acceso = 'xxxxxxxxxx'
app_secreta = 'xxxxxxx'
ad_cuenta_id = 'xxxxxxxxxxxx'

page_id = 'xxxxxxxxxx'
app_id = 'xxxxxxxxxxxx'

FacebookAdsApi.init(app_id, app_secreta, token_de_acceso)
mi_cuenta = AdAccount(ad_cuenta_id)
campaigns = mi_cuenta.get_campaigns()
print(campaigns)
```
Lanzarlo con un spark-submit:

```sh
xiafra@debian$ spark-submit test.py --master local --executor-memory 2G --executor-cores 4
```
License **TVD**



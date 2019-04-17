# new-project-windows
Manual de creación de proyecto con Windows Forms.

# Como Instalar Realm
1. Abrir Administrador de paquetes NuGet.
2. Buscar Realm e instalar
3. Crear archivo "FodyWeavers.xml" en la carpeta raiz con el siguiente contenido:
```xml
<?xml version="1.0" encoding="utf-8" ?>
<Weavers>
    <RealmWeaver />
</Weavers>
```
4. Crear archivo BaseRealm: 
```
public class BaseRealm
    {
        /// <summary>
        /// Funcion para guardar un objeto en la DB
        /// </summary>
        /// <param name="entity"></param>
        public void save(RealmObject entity)
        {
            // Obtenemos instancia de Realm
            var realm = Realm.GetInstance();
            // Guardamos en la DB
            realm.Write(() => realm.Add(entity, update: true));
        }
    }
```
5. Extender de BaseRealm:
```
public class MachineRealm : BaseRealm
    {
        /// <summary>
        /// Obtiene la maquina instalada
        /// </summary>
        /// <returns></returns>
        public Machine fetchMachine()
        {
            // Obtenemos instancia de Realm
            var realm = Realm.GetInstance();
            // Buscamos por ID de la maquina
            return realm.Find<Machine>(1);
        }
        /// <summary>
        /// Guarda el splash de la maquina
        /// </summary>
        /// <param name="splash"></param>
        /// <param name="splash_two"></param>
        public void saveSplash(string splash, string splash_two, string header)
        {
            // Obtener maquina 
            Machine machine = fetchMachine();
            // Obtenemos instancia de Realm
            var realm = Realm.GetInstance();
            // Guardar
            realm.Write(() =>
            {
                machine.splash = splash;
                machine.splash_two = splash_two;
                machine.header = header;
            });
        }

        /// <summary>
        /// Determina si existe ya instalada una maquina
        /// </summary>
        /// <returns></returns>
        public Boolean existMachine()
        {
            // Buscamos maquina
            Machine machine = fetchMachine();
            // Verificamos si existe
            if (machine == null)
            {
                return false;
            }

            return true;
        }
    }
```
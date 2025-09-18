# bludee-banco-de-sangre-pr
🩸 Sistema de banco de sangre para Puerto Rico - RBAC, multi-tenant, hub colaborativo
# 🩸 Bludee - Sistema de Banco de Sangre

Sistema integrado de gestión para bancos de sangre en Puerto Rico.

## 🚀 Características Principales

- **Sistema RBAC**: Control de roles y permisos granular
- **Multi-tenant**: Soporte para múltiples organizaciones  
- **Hub Colaborativo**: Intercambio seguro entre bancos
- **Auditoría Completa**: Trazabilidad de todas las operaciones
- **Interfaz Responsiva**: Funciona en escritorio y móvil

## 👥 Roles del Sistema

### 🏥 BANK (Banco de Sangre)
- Gestión completa de inventario
- Procesamiento de donaciones
- Despacho de unidades
- Gestión de donantes
- Acceso al hub colaborativo

### 🏥 HOSPITAL_FULL_BANK (Hospital Completo)
- Funciones de banco dentro del hospital
- Procesamiento limitado
- Gestión de solicitudes y recepciones
- Acceso a transferencias

### 🏥 HOSPITAL_RECEIVER (Hospital Receptor)
- Solo recepción de unidades
- Gestión de solicitudes
- Pruebas de compatibilidad
- Emisión de componentes

### ⚙️ ADMIN (Administrador)
- Gestión de usuarios y roles
- Auditoría completa del sistema
- Configuración de alertas
- Acceso a todos los módulos

## 🧪 Prueba el Sistema

### Opción 1: Ejecutar Localmente
```bash
# Descargar el archivo
# Ejecutar con Python
python bludee_auth.py
```

### Opción 2: Ejecutar Online (Sin Instalar)
1. Ve a [Python.org Online](https://python.org/shell/)
2. Copia y pega el código de `bludee_auth.py`
3. Ejecuta con `demo_login_system()`

### Opción 3: GitHub Codespaces
1. Abre este repo en GitHub Codespaces
2. El entorno Python estará listo automáticamente

## 🔐 Usuarios de Prueba

| Usuario | Contraseña | Rol | Organización |
|---------|------------|-----|--------------|
| `maria.garcia` | `123456` | HOSPITAL_RECEIVER | Hospital San Juan |
| `carlos.rodriguez` | `banco123` | BANK | Banco Central PR |
| `ana.lopez` | `hospital456` | HOSPITAL_FULL_BANK | Hospital Metropolitano |
| `admin` | `admin2025` | ADMIN | Sistema Bludee |

## 📁 Estructura del Proyecto

```
bludee/
├── 📄 bludee_auth.py          # Sistema de autenticación y RBAC
├── 🌐 bludee_prototype.html   # Prototipo visual interactivo
├── 📋 requirements.txt        # Dependencias Python
├── 📖 README.md              # Documentación
└── 🚧 (próximamente)
    ├── api/                   # Backend FastAPI
    ├── frontend/              # Interfaz React/Vue
    ├── database/              # Scripts de BD
    └── tests/                 # Pruebas automatizadas
```

## 🛠️ Próximos Pasos en el Desarrollo

### Fase 1: Backend (2-3 meses)
- [ ] FastAPI con endpoints REST
- [ ] Base de datos PostgreSQL
- [ ] Sistema JWT para sesiones
- [ ] APIs de inventario y solicitudes

### Fase 2: Frontend (2-3 meses)  
- [ ] Interfaz React/Vue moderna
- [ ] Responsive design
- [ ] Menú dinámico por rol
- [ ] Dashboard con métricas

### Fase 3: Hub Colaborativo (1-2 meses)
- [ ] APIs inter-organizacionales
- [ ] Sistema de consentimientos
- [ ] Rate limiting y caching
- [ ] Auditoría de transferencias

### Fase 4: Producción (1 mes)
- [ ] Despliegue en AWS/Azure
- [ ] Monitoreo y alertas
- [ ] Backup automático
- [ ] Documentación API

## 🎯 Funcionalidades Clave

### Módulo Distribución
- 📦 **Inventario**: Gestión completa de stock
- ⚗️ **Procesamiento**: Separación de componentes
- 🚚 **Despacho**: Envío a hospitales
- 👥 **Donantes**: Base de datos de donadores

### Módulo Recepción
- 📋 **Solicitudes**: Pedidos de componentes
- 📥 **Recepción**: Llegada de unidades
- 🧬 **Compatibilidad**: Pruebas cruzadas
- 💉 **Emisión**: Entrega a pacientes

### Hub Colaborativo
- 🔍 **Búsqueda**: Componentes en otros bancos
- 📤 **Compartir**: Publicar disponibilidad
- 🔄 **Transferencias**: Intercambio seguro

## 🔒 Seguridad

- **Autenticación**: Hash de contraseñas con SHA-256 (bcrypt en producción)
- **Autorización**: Sistema RBAC granular
- **Sesiones**: Tokens con expiración automática
- **Auditoría**: Log de todas las operaciones
- **Aislamiento**: Datos separados por organización

## 📊 Casos de Uso

### Hospital necesita sangre urgente
1. Hospital Receiver crea solicitud urgente
2. Sistema busca en inventario local y hub
3. Banco más cercano confirma disponibilidad
4. Se programa transferencia automática
5. Hospital recibe notificación de llegada

### Banco comparte exceso de inventario
1. Banco publica unidades próximas a expirar
2. Hospitales del hub reciben alerta
3. Hospital solicita transferencia
4. Sistema audita y autoriza intercambio
5. Unidades se transfieren antes de expirar

## 🚀 Contribuir

1. **Fork** este repositorio
2. **Crea** una rama para tu feature (`git checkout -b feature/nueva-funcionalidad`)
3. **Commit** tus cambios (`git commit -am 'Agregar nueva funcionalidad'`)
4. **Push** a la rama (`git push origin feature/nueva-funcionalidad`)
5. **Crea** un Pull Request

## 📞 Contacto

- **Desarrolladora**: Iniciativa comunitaria Puerto Rico
- **Objetivo**: Mejorar la gestión de bancos de sangre en la isla
- **Estado**: MVP en desarrollo activo

## 📄 Licencia

Este proyecto está bajo la Licencia MIT - ver el archivo LICENSE para detalles.

---
**🩸 Salvando vidas a través de la tecnología** 💙

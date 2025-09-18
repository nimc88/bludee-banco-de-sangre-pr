# 🩸 BLUDEE - SISTEMA DE LOGIN EN PYTHON
# ====================================================
# Archivo: bludee_auth.py
# Descripción: Sistema de autenticación y roles para Bludee
# Para usar en GitHub o ejecutar online
# ====================================================

import json
import hashlib
from datetime import datetime, timedelta
from typing import Dict, List, Optional, Tuple

class BludeeAuthSystem:
    """Sistema de Autenticación y Control de Roles para Bludee"""
    
    def __init__(self):
        """Inicializa el sistema con configuración de roles y usuarios"""
        # Configuración de roles y capacidades (igual que en el HTML)
        self.role_capabilities = {
            'BANK': {
                'name': 'Banco de Sangre',
                'modules': ['distribution', 'hub', 'reception'],
                'capabilities': [
                    'inventory', 'processing', 'dispatch', 'donors', 
                    'hub-search', 'hub-share', 'transfers', 'requests', 
                    'reception', 'compatibility', 'issuing'
                ]
            },
            'HOSPITAL_FULL_BANK': {
                'name': 'Hospital Completo',
                'modules': ['distribution', 'reception', 'hub'],
                'capabilities': [
                    'inventory', 'processing', 'dispatch', 'donors', 
                    'requests', 'reception', 'compatibility', 'issuing', 
                    'hub-search', 'transfers'
                ]
            },
            'HOSPITAL_RECEIVER': {
                'name': 'Hospital Receptor',
                'modules': ['reception', 'hub'],
                'capabilities': [
                    'requests', 'reception', 'compatibility', 
                    'issuing', 'hub-search'
                ]
            },
            'ADMIN': {
                'name': 'Administrador',
                'modules': ['admin', 'distribution', 'reception', 'hub'],
                'capabilities': [
                    'users', 'audit', 'alerts', 'inventory', 'processing', 
                    'dispatch', 'requests', 'reception', 'hub-search', 
                    'hub-share', 'transfers'
                ]
            }
        }
        
        # Base de datos simulada de usuarios (en producción sería una BD real)
        self.users_db = self._init_users_database()
        
        # Sesiones activas
        self.active_sessions = {}
    
    def _init_users_database(self) -> Dict:
        """Inicializa la base de datos de usuarios con datos de ejemplo"""
        # En producción, las contraseñas estarían hasheadas
        return {
            "maria.garcia": {
                "password_hash": self._hash_password("123456"),
                "name": "Dra. María García",
                "role": "HOSPITAL_RECEIVER",
                "organization": "Hospital San Juan",
                "email": "maria.garcia@hospitalsj.pr",
                "active": True,
                "created_at": "2025-01-15",
                "last_login": None
            },
            "carlos.rodriguez": {
                "password_hash": self._hash_password("banco123"),
                "name": "Dr. Carlos Rodríguez",
                "role": "BANK",
                "organization": "Banco Central PR",
                "email": "carlos@bancocentral.pr",
                "active": True,
                "created_at": "2025-01-10",
                "last_login": None
            },
            "ana.lopez": {
                "password_hash": self._hash_password("hospital456"),
                "name": "Dra. Ana López",
                "role": "HOSPITAL_FULL_BANK",
                "organization": "Hospital Metropolitano",
                "email": "ana.lopez@hosmetro.pr",
                "active": True,
                "created_at": "2025-01-20",
                "last_login": None
            },
            "admin": {
                "password_hash": self._hash_password("admin2025"),
                "name": "Administrador Sistema",
                "role": "ADMIN",
                "organization": "Sistema Bludee",
                "email": "admin@bludee.pr",
                "active": True,
                "created_at": "2025-01-01",
                "last_login": None
            }
        }
    
    def _hash_password(self, password: str) -> str:
        """Hash de contraseña usando SHA-256 (en producción usar bcrypt)"""
        return hashlib.sha256(password.encode()).hexdigest()
    
    def _generate_session_token(self, username: str) -> str:
        """Genera un token de sesión único"""
        timestamp = str(datetime.now().timestamp())
        return hashlib.sha256(f"{username}_{timestamp}".encode()).hexdigest()
    
    def authenticate_user(self, username: str, password: str) -> Tuple[bool, str, Dict]:
        """
        Autentica un usuario y devuelve el resultado
        
        Returns:
            Tuple[bool, str, Dict]: (éxito, mensaje, datos_usuario)
        """
        # Verificar si el usuario existe
        if username not in self.users_db:
            return False, "Usuario no encontrado", {}
        
        user_data = self.users_db[username]
        
        # Verificar si el usuario está activo
        if not user_data["active"]:
            return False, "Usuario desactivado", {}
        
        # Verificar contraseña
        password_hash = self._hash_password(password)
        if password_hash != user_data["password_hash"]:
            return False, "Contraseña incorrecta", {}
        
        # Actualizar último login
        self.users_db[username]["last_login"] = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        
        # Crear sesión
        session_token = self._generate_session_token(username)
        session_data = {
            "username": username,
            "role": user_data["role"],
            "organization": user_data["organization"],
            "login_time": datetime.now(),
            "expires_at": datetime.now() + timedelta(hours=8)  # 8 horas de sesión
        }
        
        self.active_sessions[session_token] = session_data
        
        # Preparar datos para devolver
        user_info = {
            "username": username,
            "name": user_data["name"],
            "role": user_data["role"],
            "organization": user_data["organization"],
            "email": user_data["email"],
            "session_token": session_token,
            "capabilities": self.role_capabilities[user_data["role"]]["capabilities"],
            "modules": self.role_capabilities[user_data["role"]]["modules"]
        }
        
        return True, "Login exitoso", user_info
    
    def check_permission(self, session_token: str, required_capability: str) -> bool:
        """Verifica si un usuario tiene permiso para realizar una acción"""
        # Verificar si la sesión existe y es válida
        if session_token not in self.active_sessions:
            return False
        
        session = self.active_sessions[session_token]
        
        # Verificar si la sesión ha expirado
        if datetime.now() > session["expires_at"]:
            del self.active_sessions[session_token]
            return False
        
        # Obtener capacidades del rol del usuario
        user_role = session["role"]
        user_capabilities = self.role_capabilities[user_role]["capabilities"]
        
        return required_capability in user_capabilities
    
    def get_user_menu(self, session_token: str) -> List[Dict]:
        """Genera el menú dinámico basado en el rol del usuario"""
        if session_token not in self.active_sessions:
            return []
        
        session = self.active_sessions[session_token]
        user_role = session["role"]
        role_config = self.role_capabilities[user_role]
        
        menu_items = []
        
        # Módulo Distribución
        if 'distribution' in role_config["modules"]:
            distribution_items = []
            if 'inventory' in role_config["capabilities"]:
                distribution_items.append({"id": "inventory", "name": "Inventario", "icon": "📦"})
            if 'processing' in role_config["capabilities"]:
                distribution_items.append({"id": "processing", "name": "Procesamiento", "icon": "⚗️"})
            if 'dispatch' in role_config["capabilities"]:
                distribution_items.append({"id": "dispatch", "name": "Despacho", "icon": "🚚"})
            if 'donors' in role_config["capabilities"]:
                distribution_items.append({"id": "donors", "name": "Donantes", "icon": "👥"})
            
            if distribution_items:
                menu_items.append({
                    "section": "distribution",
                    "title": "🏥 Módulo Distribución",
                    "items": distribution_items
                })
        
        # Módulo Recepción
        if 'reception' in role_config["modules"]:
            reception_items = []
            if 'requests' in role_config["capabilities"]:
                reception_items.append({"id": "requests", "name": "Solicitudes", "icon": "📋"})
            if 'reception' in role_config["capabilities"]:
                reception_items.append({"id": "reception", "name": "Recepción", "icon": "📥"})
            if 'compatibility' in role_config["capabilities"]:
                reception_items.append({"id": "compatibility", "name": "Compatibilidad", "icon": "🧬"})
            if 'issuing' in role_config["capabilities"]:
                reception_items.append({"id": "issuing", "name": "Emisión", "icon": "💉"})
            
            if reception_items:
                menu_items.append({
                    "section": "reception", 
                    "title": "🏥 Módulo Recepción",
                    "items": reception_items
                })
        
        # Hub Colaborativo
        if 'hub' in role_config["modules"]:
            hub_items = []
            if 'hub-search' in role_config["capabilities"]:
                hub_items.append({"id": "hub-search", "name": "Buscar Componentes", "icon": "🔍"})
            if 'hub-share' in role_config["capabilities"]:
                hub_items.append({"id": "hub-share", "name": "Compartir Inventario", "icon": "📤"})
            if 'transfers' in role_config["capabilities"]:
                hub_items.append({"id": "transfers", "name": "Transferencias", "icon": "🔄"})
            
            if hub_items:
                menu_items.append({
                    "section": "hub",
                    "title": "🌐 Hub Colaborativo", 
                    "items": hub_items
                })
        
        # Administración
        if 'admin' in role_config["modules"]:
            admin_items = []
            if 'users' in role_config["capabilities"]:
                admin_items.append({"id": "users", "name": "Usuarios", "icon": "👤"})
            if 'audit' in role_config["capabilities"]:
                admin_items.append({"id": "audit", "name": "Auditoría", "icon": "📊"})
            if 'alerts' in role_config["capabilities"]:
                admin_items.append({"id": "alerts", "name": "Alertas", "icon": "⚠️"})
            
            if admin_items:
                menu_items.append({
                    "section": "admin",
                    "title": "⚙️ Administración",
                    "items": admin_items
                })
        
        return menu_items
    
    def logout_user(self, session_token: str) -> bool:
        """Cierra la sesión de un usuario"""
        if session_token in self.active_sessions:
            del self.active_sessions[session_token]
            return True
        return False
    
    def get_session_info(self, session_token: str) -> Optional[Dict]:
        """Obtiene información de la sesión actual"""
        if session_token in self.active_sessions:
            session = self.active_sessions[session_token]
            if datetime.now() <= session["expires_at"]:
                return session
            else:
                # Sesión expirada
                del self.active_sessions[session_token]
        return None


# ====================================================
# 🧪 FUNCIONES DE TESTING Y DEMOSTRACIÓN
# ====================================================

def demo_login_system():
    """Función para demostrar el sistema de login"""
    print("🩸 BLUDEE - DEMOSTRACIÓN DEL SISTEMA DE LOGIN")
    print("=" * 50)
    
    # Crear instancia del sistema
    auth_system = BludeeAuthSystem()
    
    # Mostrar usuarios disponibles
    print("\n👥 USUARIOS DE PRUEBA:")
    users_info = [
        ("maria.garcia", "123456", "HOSPITAL_RECEIVER"),
        ("carlos.rodriguez", "banco123", "BANK"),
        ("ana.lopez", "hospital456", "HOSPITAL_FULL_BANK"),
        ("admin", "admin2025", "ADMIN")
    ]
    
    for username, password, role in users_info:
        print(f"  Usuario: {username} | Contraseña: {password} | Rol: {role}")
    
    print("\n" + "=" * 50)
    
    # Pruebas de login
    test_cases = [
        ("maria.garcia", "123456", True),
        ("carlos.rodriguez", "contraseña_incorrecta", False),
        ("usuario_inexistente", "123456", False),
        ("admin", "admin2025", True)
    ]
    
    for username, password, should_succeed in test_cases:
        print(f"\n🔐 Probando login: {username}")
        success, message, user_data = auth_system.authenticate_user(username, password)
        
        if success:
            print(f"  ✅ {message}")
            print(f"     Nombre: {user_data['name']}")
            print(f"     Organización: {user_data['organization']}")
            print(f"     Rol: {user_data['role']}")
            
            # Mostrar menú dinámico
            session_token = user_data['session_token']
            menu = auth_system.get_user_menu(session_token)
            print(f"     Menú disponible:")
            for section in menu:
                print(f"       {section['title']}")
                for item in section['items']:
                    print(f"         - {item['icon']} {item['name']}")
            
            # Probar algunos permisos
            print(f"     Permisos:")
            test_permissions = ['inventory', 'dispatch', 'donors', 'requests', 'users']
            for perm in test_permissions:
                has_perm = auth_system.check_permission(session_token, perm)
                status = "✅" if has_perm else "❌"
                print(f"       {status} {perm}")
                
        else:
            print(f"  ❌ {message}")


def create_github_files():
    """Crea archivos adicionales para el repositorio de GitHub"""
    
    # README.md
    readme_content = """# 🩸 Bludee - Sistema de Banco de Sangre

Sistema integrado de gestión para bancos de sangre en Puerto Rico.

## 🚀 Características

- **Sistema RBAC**: Control de roles y permisos granular
- **Multi-tenant**: Soporte para múltiples organizaciones
- **Hub Colaborativo**: Intercambio seguro entre bancos
- **Auditoría Completa**: Trazabilidad de todas las operaciones

## 👥 Roles del Sistema

- **BANK**: Banco de sangre completo
- **HOSPITAL_FULL_BANK**: Hospital con capacidades de banco
- **HOSPITAL_RECEIVER**: Hospital receptor solamente
- **ADMIN**: Administrador del sistema

## 🧪 Prueba el Sistema

```python
python bludee_auth.py
```

## 📁 Estructura del Proyecto

```
bludee/
├── bludee_auth.py          # Sistema de autenticación
├── bludee_prototype.html   # Prototipo visual
├── requirements.txt        # Dependencias
└── README.md              # Este archivo
```

## 🛠️ Próximos Pasos

1. Implementar FastAPI backend
2. Base de datos PostgreSQL
3. Frontend React/Vue
4. Despliegue en la nube
"""

    # requirements.txt
    requirements_content = """# Dependencias para Bludee
fastapi==0.104.1
uvicorn==0.24.0
sqlalchemy==2.0.23
psycopg2-binary==2.9.9
python-jose[cryptography]==3.3.0
python-multipart==0.0.6
bcrypt==4.0.1
pydantic==2.5.0
python-dotenv==1.0.0
"""

    return readme_content, requirements_content


# ====================================================
# 🎯 EJECUTAR DEMOSTRACIÓN
# ====================================================

if __name__ == "__main__":
    # Ejecutar la demostración
    demo_login_system()
    
    print("\n" + "=" * 50)
    print("📁 ARCHIVOS PARA GITHUB CREADOS:")
    print("  - bludee_auth.py (este archivo)")
    print("  - README.md")
    print("  - requirements.txt")
    print("\n¡Listo para subir a GitHub! 🚀")

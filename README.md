# AeroChain

**Decentralized Flight Reservation Application on Solana**

AeroChain es una aplicación descentralizada de reservación de vuelos construida sobre **Solana**, utilizando **Rust, Anchor y TypeScript**. Permite conectar una wallet, consultar vuelos disponibles, crear vuelos y reservar asientos mediante transacciones registradas en la blockchain.

El proyecto integra un programa on-chain desarrollado con Anchor y una interfaz web que permite interactuar con dicho programa desde una wallet compatible con Solana.

<img width="1080" height="1080" alt="AeroChain" src="https://github.com/user-attachments/assets/4ee0f914-c5d8-453e-87c1-c45741623e1e" />

---

## 1. Arquitectura del proyecto

AeroChain utiliza una arquitectura cliente-blockchain. Las reglas de negocio y las operaciones principales se ejecutan mediante un programa desplegado en Solana Devnet.

| Componente | Tecnología | Responsabilidad |
|---|---|---|
| Programa on-chain | Rust + Anchor | Lógica de vuelos y reservaciones |
| Blockchain | Solana Devnet | Registro de cuentas y transacciones |
| Cuentas | Program Derived Addresses (PDAs) | Almacenamiento de información on-chain |
| IDL | Anchor IDL | Interfaz entre el programa y el cliente |
| Cliente | TypeScript + Anchor | Comunicación con el programa |
| Frontend | TypeScript + Vite | Interfaz de usuario |
| Wallet | Phantom | Conexión y firma de transacciones |

### Flujo general de interacción

1. El usuario abre AeroChain desde el navegador.
2. Conecta su wallet Phantom.
3. La aplicación establece comunicación con Solana Devnet.
4. El cliente utiliza el IDL para interactuar con el programa Anchor.
5. El usuario puede consultar vuelos, crear vuelos o reservar asientos.
6. Las operaciones que modifican el estado de la blockchain requieren una transacción firmada por la wallet.
7. La interfaz actualiza la información de vuelos y reservaciones.

---

## 2. Funcionalidades implementadas

### Conexión de wallet

- Conexión con Phantom.
- Identificación de la dirección pública de la wallet.
- Uso de la wallet conectada para firmar transacciones.
- Consulta de información asociada al usuario.

### Administración de vuelos

- Creación de vuelos directamente en Solana Devnet.
- Registro de información del vuelo mediante cuentas on-chain.
- Consulta de vuelos disponibles.
- Visualización de información como número de vuelo, origen y destino.

### Reservación de asientos

- Selección de un vuelo disponible.
- Captura del nombre del pasajero.
- Selección de un asiento.
- Creación de una reservación mediante una transacción on-chain.
- Asociación de la reservación con el vuelo y la wallet correspondiente.
- Consulta de las reservaciones realizadas por la wallet conectada.

### Control de ocupación

La aplicación consulta el estado de los asientos registrados en la blockchain.

- Los asientos ocupados se identifican en la interfaz.
- No se permite seleccionar un asiento que ya se encuentra ocupado.
- Las diferentes wallets consultan el mismo estado global de ocupación del vuelo.

### Actualización automática

AeroChain incorpora un mecanismo de actualización automática de vuelos cada **15 segundos**.

Esta funcionalidad permite que diferentes usuarios visualicen los cambios en la ocupación de los asientos sin necesidad de recargar manualmente la página.

Además, la aplicación evita actualizar los formularios mientras el usuario está interactuando con ellos, preservando la información que está escribiendo.

---

## 3. Programa desplegado en Solana Devnet

El programa de AeroChain se encuentra desplegado en Solana Devnet.

**Program ID:**

```text
E3Ai1odomZQjyXAbvy1bRpXuvXATELeYKZ7HVsShvg2C
```

**Network:** Solana Devnet

Las operaciones de creación de vuelos y reservación de asientos generan transacciones reales en la red de pruebas de Solana.

Esto permite verificar las operaciones directamente en la blockchain y comprobar que la aplicación no depende exclusivamente de información almacenada en el navegador.

---

## 4. Modelo de funcionamiento de las reservaciones

El funcionamiento de AeroChain distingue dos tipos de información:

**Vuelos disponibles**

Los vuelos creados en la blockchain pueden ser consultados por diferentes usuarios, independientemente de la wallet que tengan conectada.

**Mis reservaciones**

La sección de reservaciones consulta las cuentas asociadas a la wallet actualmente conectada.

Por lo tanto, cada usuario puede consultar sus propias reservaciones, mientras que la ocupación de los asientos es compartida entre todos los usuarios.

Este comportamiento permite que dos usuarios conectados con wallets diferentes observen el mismo estado de un vuelo, aunque cada uno tenga un historial de reservaciones independiente.

---

## 5. Pruebas funcionales realizadas

Durante el desarrollo se realizaron pruebas de integración entre el programa Anchor, el cliente TypeScript, Phantom y Solana Devnet.

| Prueba | Resultado |
|---|---|
| Conexión con Phantom | Verificada |
| Identificación de wallet | Verificada |
| Consulta de vuelos | Verificada |
| Creación de vuelo on-chain | Verificada |
| Firma de transacciones | Verificada |
| Reservación de asiento | Verificada |
| Registro de reservación en Devnet | Verificado |
| Consulta de Mis Reservaciones | Verificada |
| Visualización de asientos ocupados | Verificada |
| Actualización entre wallets diferentes | Verificada |
| Protección de formularios durante actualización automática | Verificada |
| Compilación del frontend | Correcta |
| TypeScript typecheck | Correcto |

### Prueba de actualización entre usuarios

Se conectaron dos wallets Phantom diferentes desde sesiones independientes del navegador.

El procedimiento permitió comprobar que:

1. La primera wallet creó un vuelo.
2. La segunda wallet pudo consultar ese vuelo.
3. La segunda wallet no mostró reservaciones pertenecientes a la primera.
4. La primera wallet realizó una reservación.
5. La segunda wallet visualizó el asiento ocupado después de la actualización automática, sin recargar la página ni utilizar manualmente el botón de actualización.

Esta prueba confirmó que el estado de ocupación se comparte a través de Solana Devnet y no depende de la sesión local del navegador.

---

## 6. Tecnologías y herramientas

- **Rust:** desarrollo de la lógica del programa on-chain.
- **Anchor Framework:** estructura, cuentas e instrucciones del programa Solana.
- **Solana:** infraestructura blockchain.
- **Solana Devnet:** red de pruebas y despliegue.
- **TypeScript:** lógica del cliente y frontend.
- **Vite:** servidor de desarrollo y compilación del frontend.
- **Phantom Wallet:** conexión y firma de transacciones.
- **IDL:** definición de la interfaz del programa Anchor.
- **Git y GitHub:** control de versiones y administración del código fuente.
- **GitHub Codespaces:** entorno de desarrollo.

---

## 7. Ejecución y desarrollo

### Requisitos

Para trabajar con el proyecto se requiere:

- Rust y Cargo.
- Solana CLI.
- Anchor Framework.
- Node.js.
- Yarn.
- Una wallet Phantom configurada para Solana Devnet.

### Obtener el proyecto

```bash
git clone https://github.com/JuanFMaltos13/AeroChain.git
cd AeroChain
```

### Instalar dependencias

```bash
yarn install
```

### Compilar el programa Anchor

```bash
anchor build
```

### Validar el frontend

```bash
yarn typecheck:app
yarn build
```

### Ejecutar el entorno de desarrollo

```bash
yarn dev
```

El frontend utiliza el puerto 5173 durante el desarrollo.

**Importante:** Para ejecutar transacciones en Devnet, Phantom debe estar configurada para utilizar la misma red y disponer de SOL de prueba para cubrir las comisiones.

---

## 8. Estado actual y trabajo futuro

AeroChain cuenta con un flujo funcional de creación de vuelos, conexión de wallet, reservación de asientos y consulta de ocupación compartida entre usuarios.

Las siguientes mejoras pueden ampliar el alcance del proyecto:

- Mejorar la presentación de los estados de las transacciones.
- Mostrar información más detallada de cada reservación.
- Incorporar mecanismos de cancelación de reservaciones.
- Implementar modificaciones y administración de vuelos.
- Ampliar las pruebas de capacidad y concurrencia.
- Mejorar la experiencia de usuario y el diseño responsive.
- Incorporar documentación adicional de las cuentas, instrucciones y PDAs.

---

## 9. Objetivo del proyecto

AeroChain demuestra la integración de desarrollo de software y tecnología blockchain mediante una aplicación descentralizada que permite ejecutar operaciones reales sobre Solana.

El objetivo es construir una solución funcional, verificable y extensible, aplicando conceptos de programación en Rust, desarrollo de smart contracts, integración con wallets, arquitectura cliente-blockchain y control de versiones.

**Repository:** [github.com/JuanFMaltos13/AeroChain](https://github.com/JuanFMaltos13/AeroChain)

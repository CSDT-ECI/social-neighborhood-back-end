# Informe de Deuda Técnica

A continuación, se presenta un análisis detallado de la deuda técnica en el repositorio **social-neighborhood-back-end**. Este informe incluye problemas arquitectónicos, violaciones de principios de diseño, code smells, riesgos de mantenibilidad, problemas de testabilidad y estrategias de refactorización sugeridas.

---

#### 1. **Problemas Arquitectónicos**
- **Acoplamiento excesivo entre capas**: Las clases de servicios como [`AdminServices`](src/main/java/edu/eci/arsw/socialneighborhood/services/AdminServices.java) y [`CommonServices`](src/main/java/edu/eci/arsw/socialneighborhood/services/CommonServices.java) dependen directamente de implementaciones específicas como [`SocialNeighborhoodImpl`](src/main/java/edu/eci/arsw/socialneighborhood/persistence/impl/SocialNeighborhoodImpl.java), lo que dificulta la sustitución o modificación de estas implementaciones.
- **Falta de separación de responsabilidades**: Algunas clases como [`CacheItemAdmin`](src/main/java/edu/eci/arsw/socialneighborhood/cache/cacheItem/CacheItemAdmin.java) combinan lógica de negocio con lógica de caché, violando el principio de separación de preocupaciones.
- **Uso inconsistente de anotaciones de Spring**: En varias clases, como [`SocialNeighborhoodImpl`](src/main/java/edu/eci/arsw/socialneighborhood/persistence/impl/SocialNeighborhoodImpl.java), se mezclan anotaciones como `@Service` y `@Component`, lo que puede generar confusión sobre su propósito.

---

#### 2. **Violaciones de Principios de Diseño**
- **Violación del principio de responsabilidad única (SRP)**:
  - Clases como [`AdminController`](src/main/java/edu/eci/arsw/socialneighborhood/controller/AdminController.java) manejan múltiples responsabilidades, desde la validación de datos hasta la lógica de negocio.
  - [`CacheItemAdmin`](src/main/java/edu/eci/arsw/socialneighborhood/cache/cacheItem/CacheItemAdmin.java) mezcla la lógica de caché con la obtención de datos.
- **Violación del principio de inversión de dependencias (DIP)**:
  - Las clases de servicios dependen directamente de repositorios específicos como [`AlquilerRepository`](src/main/java/edu/eci/arsw/socialneighborhood/repository/AlquilerRepository.java), en lugar de abstraer estas dependencias.
- **Violación del principio DRY (Don't Repeat Yourself)**:
  - Métodos como `getTipoInmuebleConjuntoById` y `getTipoAgrupacionConjuntoById` en [`CommonServices`](src/main/java/edu/eci/arsw/socialneighborhood/services/CommonServices.java) tienen lógica redundante.
- **Violación del principio KISS (Keep It Simple, Stupid)**:
  - Métodos como `postAlquiler` en [`SocialNeighborhoodImpl`](src/main/java/edu/eci/arsw/socialneighborhood/persistence/impl/SocialNeighborhoodImpl.java) contienen lógica compleja y difícil de seguir.

---

#### 3. **Code Smells**
- **Clases grandes (God Classes)**:
  - [`AdminController`](src/main/java/edu/eci/arsw/socialneighborhood/controller/AdminController.java) y [`SocialNeighborhoodImpl`](src/main/java/edu/eci/arsw/socialneighborhood/persistence/impl/SocialNeighborhoodImpl.java) contienen demasiados métodos, lo que indica una sobrecarga de responsabilidades.
- **Métodos largos**:
  - Métodos como `actualizaCacheAdmin` en [`CacheItemAdmin`](src/main/java/edu/eci/arsw/socialneighborhood/cache/cacheItem/CacheItemAdmin.java) son difíciles de leer y mantener.
- **Nombres de métodos poco descriptivos**:
  - Métodos como `getAgrupacion` o `getConjuntos` en [`SocialNeighborhoodImpl`](src/main/java/edu/eci/arsw/socialneighborhood/persistence/impl/SocialNeighborhoodImpl.java) no explican claramente su propósito.
- **Uso excesivo de literales mágicos**:
  - En consultas SQL como las de [`AlquilerRepository`](src/main/java/edu/eci/arsw/socialneighborhood/repository/AlquilerRepository.java), se utilizan valores literales que deberían ser constantes.

---

#### 4. **Riesgos de Mantenibilidad**
- **Falta de documentación**: Muchas clases y métodos carecen de comentarios que expliquen su propósito o funcionamiento.
- **Dependencias rígidas**: La falta de interfaces para repositorios y servicios dificulta la introducción de cambios sin afectar otras partes del sistema.
- **Falta de modularidad**: La lógica de negocio está dispersa en múltiples capas, lo que dificulta la localización de errores o la implementación de nuevas características.

---

#### 5. **Problemas de Testabilidad**
- **Falta de inyección de dependencias adecuada**: Aunque se utiliza Spring, algunas dependencias no están completamente desacopladas, lo que dificulta la creación de pruebas unitarias.
- **Cobertura de pruebas insuficiente**: No se observan indicios claros de pruebas unitarias o de integración en el repositorio.
- **Métodos con múltiples responsabilidades**: Métodos como `putUnidadDeViviendaUsuario` en [`SocialNeighborhoodImpl`](src/main/java/edu/eci/arsw/socialneighborhood/persistence/impl/SocialNeighborhoodImpl.java) son difíciles de probar debido a su complejidad.

---

#### 6. **Estrategias de Refactorización**
- **Aplicar el principio de inversión de dependencias (DIP)**:
  - Crear interfaces para repositorios y servicios, y utilizarlas en lugar de depender directamente de implementaciones concretas.
- **Dividir clases grandes**:
  - Separar las responsabilidades de clases como [`AdminController`](src/main/java/edu/eci/arsw/socialneighborhood/controller/AdminController.java) en controladores más pequeños y específicos.
- **Simplificar métodos complejos**:
  - Dividir métodos largos como `actualizaCacheAdmin` en métodos más pequeños y reutilizables.
- **Eliminar duplicación de código**:
  - Consolidar lógica redundante en métodos reutilizables o utilitarios.
- **Agregar pruebas unitarias**:
  - Implementar pruebas para cubrir casos de uso clave y garantizar la estabilidad del sistema.
- **Documentar el código**:
  - Agregar comentarios claros y consistentes en clases y métodos para mejorar la comprensión del sistema.

---

#### **Conclusión**
El repositorio presenta una arquitectura funcional pero con áreas de mejora significativas en términos de diseño, mantenibilidad y testabilidad. La implementación de las estrategias de refactorización sugeridas reducirá la deuda técnica y mejorará la calidad general del sistema.

---
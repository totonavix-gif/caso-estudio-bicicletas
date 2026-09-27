# Caso de Estudio: Optimización del Uso de Bicicletas Compartidas
**Proyecto Final - Certificado Profesional de Análisis de Datos de Google**

## 1. Fase: Preguntar (Ask)
El objetivo de este proyecto es analizar cómo difieren los hábitos de uso de las bicicletas compartidas entre los diferentes tipos de usuarios del servicio. 

### Preguntas clave de negocio:
* ¿Qué tipos de suscriptores generan el mayor volumen de viajes?
* ¿Existe una preferencia marcada por el tipo de bicicleta (eléctrica vs. clásica)?
* ¿Cómo podemos utilizar estos hallazgos para diseñar estrategias de marketing que conviertan a los usuarios ocasionales en miembros anuales recurrentes?

---

## 2. Fase: Preparar (Prepare)
Para este análisis se utilizó el dataset histórico de viajes del sistema de bicicletas compartidas, ubicado en la carpeta `data_raw/` de este repositorio. 

### Evaluación de la calidad de los datos (Criterio ROCCC de Google):
* **Confiables (Reliable):** Los datos provienen directamente del registro automatizado del sistema de bicicletas.
* **Originales (Original):** Es una fuente primaria de datos del servicio público de transporte.
* **Completos (Comprehensive):** Contiene información desagregada por tipo de suscripción y tipo de vehículo.
* **Actuales (Current):** Refleja las tendencias operativas recientes del sistema.
* **Citados (Cited):** Los datos se manejan bajo estricto anonimato de los usuarios, cumpliendo con las normativas de privacidad vigentes.


---

## 5. Fase: Compartir (Share)
Para comunicar los hallazgos de forma visual e impactante a las partes interesadas, se desarrollaron gráficos automatizados en **R** utilizando la librería `ggplot2`. Estos gráficos permiten identificar de un vistazo las tendencias de consumo.

```r
# Gráfico 1: Preferencia Absoluta de Bicicletas Eléctricas
ggplot(tabla_bicicletas, aes(x = reorder(tipo_de_bicicleta, -proporcion), y = proporcion, fill = tipo_de_bicicleta)) +
  geom_bar(stat = "identity", width = 0.6) +
  labs(title = "Dominancia del Tipo de Bicicleta en el Servicio", x = "Tipo de Bicicleta", y = "Porcentaje de Uso (%)") +
  theme_minimal()

# Gráfico 2: Distribución de Viajes por Suscriptor (Top 5)
viajes_limpios %>%
  count(tipo_de_suscriptor) %>%
  top_n(5) %>%
  ggplot(aes(x = reorder(tipo_de_suscriptor, n), y = n, fill = tipo_de_suscriptor)) +
  geom_col() +
  coord_flip() +
  labs(title = "Top 5 Tipos de Suscriptores con Mayor Volumen", x = "Tipo de Suscriptor", y = "Total de Viajes") +
  theme_minimal()
```

---

## 6. Fase: Actuar (Act)
Basado en el análisis cuantitativo de los 5,000 viajes registrados en el sistema, comparto las siguientes **3 recomendaciones estratégicas de negocio** para la junta directiva con el fin de convertir usuarios ocasionales en miembros anuales:

1. **Campaña de Conversión Eléctrica:** Dado que las bicicletas eléctricas dominan de manera absoluta el **75.28%** del mercado, se debe crear una membresía premium anual enfocada exclusivamente en este segmento (por ejemplo: "Plan Local Electrónico"), ofreciendo minutos gratis de electricidad a los usuarios que migren de "Pago por viaje" o "Viaje individual".
2. **Estrategia de Fin de Semana:** Los usuarios de tipo **Explorador** y **Fin de semana de 3 días** representan juntos casi el **28.4%** de la demanda total de la plataforma. Se recomienda lanzar cupones de descuento directos hacia membresías anuales que se activen automáticamente el lunes por la mañana para retener a este volumen masivo de usuarios de fin de semana.
3. **Rediseño del Plan Estudiantil:** Las membresías estudiantiles (incluyendo las de la UT) apenas alcanzan cerca del **10%** del uso total de la plataforma. Para incentivar este mercado y dar salida al inventario rezagado de bicicletas clásicas (**24.72%**), se propone lanzar un plan estudiantil de bajo costo limitado exclusivamente al uso de bicicletas clásicas en días hábiles.

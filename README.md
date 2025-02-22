import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

file_path = "Readmissions_and_Deaths_-_Hospital.xlsx"  # Asegúrate de que el archivo esté en la misma carpeta del script
df = pd.read_excel(file_path)

print(df.head())
print(df.info())

print(df.isnull().sum())
print(df.dtypes)
df["Score"] = pd.to_numeric(df["Score"], errors="coerce")
print(df["Compared to National"].unique())
print(df["Measure Name"].unique())

df["Denominator"] = pd.to_numeric(df["Denominator"], errors="coerce")

sns.set_theme(style="whitegrid")

 
plt.figure(figsize=(8,5))
sns.histplot(df["Score"].dropna(), bins=30, kde=True, color="royalblue")
plt.title("Distribución de Score (Tasa de Readmisión/Mortalidad)")
plt.xlabel("Score")
plt.ylabel("Frecuencia")
plt.show()

 
plt.figure(figsize=(12,6))
top_states = df["State"].value_counts().head(10).index  # Solo los 10 estados con más datos
sns.boxplot(data=df[df["State"].isin(top_states)], x="State", y="Score", palette="coolwarm")
plt.title("Distribución de Score por Estado (Top 10)")
plt.xlabel("Estado")
plt.ylabel("Score")
plt.xticks(rotation=45)
plt.show()


plt.figure(figsize=(10,6))
sns.scatterplot(data=df, x="Denominator", y="Score", hue="Compared to National", alpha=0.7, palette="Set1")
plt.title("Relación entre Score y Denominator, categorizado por desempeño")
plt.xlabel("Denominator (Cantidad de Casos Evaluados)")
plt.ylabel("Score (Tasa de Readmisión/Mortalidad)")
plt.xscale("log")  # Escala logarítmica para mejor visualización
plt.legend(title="Comparado con el Nacional")
plt.show()



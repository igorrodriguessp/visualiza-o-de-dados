
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns
from tabulate import tabulate
import numpy as np

df = pd.read_csv('ecommerce_preparados.csv')


#print(df["Gênero"])

print('Analise de dados únicos\n', df.nunique())

print(df.head(20).to_string())

# Filtrar dados não nulos na coluna 'Desconto' > + LIMPEZA DE DADOS
desconto_validos = df['Desconto'].dropna()

# Criar o histograma para a coluna 'Desconto'
plt.figure(figsize=(8, 6))
plt.hist(desconto_validos, bins=20, edgecolor='black', color='skyblue')

# Adicionar título e rótulos aos eixos
plt.title('Distribuição de Descontos nos Produtos', fontsize=14)
plt.xlabel('Desconto (%)', fontsize=12)
plt.ylabel('Frequência', fontsize=12)

# Exibir o gráfico
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.show()


# Filtrar dados não nulos na coluna 'Nota' > + LIMPEZA DE DADOS
notas_validas = df['Nota'].dropna()

# histograma DE NOTA > Ele verifica classifição dos produtos
plt.hist(df['Nota'])
plt.hist(notas_validas, bins=100, color='green', alpha=0.8)
plt.title('histograma - Distribuição de Nota')
plt.xlabel('Nota')
plt.ylabel('frequencia')
plt.grid(True)
plt.show()


# Filtrar dados não nulos na coluna 'Nota' > + LIMPEZA DE DADOS
qtd_vendidos_validas = df['Qtd_Vendidos'].dropna()



plt.subplot(1, 2, 2) #1 linha, 2 colunas, 2* grafico
plt.scatter(df['Preço'], df["Temporada_Cod"], color="#5883a8", alpha=0.6, s=30)  #cor hexadecimal online
plt.title('dispersão - Preço e Temporada')
plt.xlabel('Preço')
plt.ylabel('Temporada')



#mapa de calor
corr = df[['Preço', 'Temporada_Cod']].corr()
plt.subplot(2, 2, 3) #1 linha, 2 colunas, 3* grafico
sns.heatmap(corr, annot=True, cmap='coolwarm')
plt.title('correlação Preço e Temporada')


plt.tight_layout() # ajustar espaçamento
plt.show()



# Agrupar categorias com menos de 5% em 'Outros'
df_genero = df['Gênero'].value_counts(normalize=True)
df_genero['Outros'] = df_genero[df_genero < 0.05].sum()
df_genero = df_genero[df_genero >= 0.05]


x = df_genero.index
y = df_genero.values


plt.figure(figsize=(10, 6))
plt.pie(y, labels=None, autopct="%.1f%%", startangle=90)
plt.legend(x, title="Gênero", loc="center left", bbox_to_anchor=(1, 0.5))
plt.title('Distribuição de Gênero')
plt.tight_layout()
plt.show()

#gráfico de pizza com legenda
'''
Se as categorias são muitas, considere usar um gráfico de barras horizontais
, que é mais adequado para representar dados categóricos com muitos valores:
'''
plt.figure(figsize=(12, 8))
df['Gênero'].value_counts().plot(kind='barh', color='skyblue')
plt.title('Distribuição de Gênero')
plt.xlabel('Frequência')
plt.ylabel('Gênero')
plt.tight_layout()
plt.show()


top_marcas = df.groupby('Marca')['Qtd_Vendidos_Cod'].sum().nlargest(10)

top_marcas.plot(kind='bar', color='skyblue', figsize=(10, 6))
plt.title('Top 10 Marcas por Quantidade Vendida')
plt.ylabel('Quantidade Vendida')
plt.xlabel('Marcas')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()


# Filtrar dados não nulos na coluna 'Nota' > + LIMPEZA DE DADOS
qtd_preco = df['Preço'].dropna()

#Densidade
plt.figure(figsize=(10, 6))
sns.kdeplot(qtd_preco, fill=True, color='#863e9c')  # Use a variável filtrada
plt.title('Densidade de Preço')
plt.xlabel('Preço')
plt.show()



#grafico de regressão
sns.regplot(x='Desconto', y='Qtd_Vendidos_Cod', data=df, color='#278f65', scatter_kws={'alpha': 0.5, 'color': '#34c289'})
plt.title('regressão de Qtd vendidos por Desconto')
plt.xlabel('Desconto')
plt.ylabel('Qtd_Vendidos_Cod')
plt.show()



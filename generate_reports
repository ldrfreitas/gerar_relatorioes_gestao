import pandas as pd
import os

def formatar_valor(valor):
    return f'R${valor:,.2f}'.replace(',', '.').replace('.', ',')

def gerar_relatorios():
    anos = [2022, 2023, 2024]
    arquivos = [f'planilha_pagamento_{ano}.csv' for ano in anos if os.path.exists(f'planilha_pagamento_{ano}.csv')]

    dfs = []
    for arquivo in arquivos:
        ano = int(arquivo.split('_')[-1].split('.')[0])
        df = pd.read_csv(arquivo)
        df['Ano'] = ano 
        dfs.append(df)
        
    df_total = pd.concat(dfs, ignore_index=True)

    df_total['Data de Pagamento'] = pd.to_datetime(df_total['Data de Pagamento'])

    df_total['Mês'] = df_total['Data de Pagamento'].dt.month

    relatorio_mensal_planos = df_total.groupby(['Ano', 'Mês', 'Plano']).agg(
        Quantidade_de_Pessoas=('Nome', 'count'),
        Valor_Total=('Valor', 'sum')
    ).reset_index()
    relatorio_mensal_planos['Valor_Total'] = relatorio_mensal_planos['Valor_Total'].apply(formatar_valor)
    relatorio_mensal_planos.to_csv('relatorio_mensal_planos.csv', index=False)
    print("\nRelatório Mensal por Plano:")
    print(relatorio_mensal_planos)

    relatorio_anual_planos = df_total.groupby(['Ano', 'Plano']).agg(
        Quantidade_de_Pessoas=('Nome', 'count'),
        Valor_Total=('Valor', 'sum')
    ).reset_index()
    relatorio_anual_planos['Valor_Total'] = relatorio_anual_planos['Valor_Total'].apply(formatar_valor)
    relatorio_anual_planos.to_csv('relatorio_anual_planos.csv', index=False)
    print("\nRelatório Anual por Plano:")
    print(relatorio_anual_planos)

    resumo_planos = df_total.groupby('Plano').agg(
        Quantidade_de_Pessoas=('Nome', 'count'),
        Valor_Total=('Valor', 'sum')
    ).reset_index()
    resumo_planos['Valor_Total'] = resumo_planos['Valor_Total'].apply(formatar_valor)
    print("\nResumo por Plano:")
    for _, row in resumo_planos.iterrows():
        print(f"{row['Quantidade_de_Pessoas']} pessoas adotam o {row['Plano']} com valor total de {row['Valor_Total']}")

gerar_relatorios()

print("\nRelatórios gerados com sucesso.")

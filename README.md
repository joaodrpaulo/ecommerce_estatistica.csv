import pandas as pd
import dash
from dash import dcc, html
import plotly.express as px

# Leitura do arquivo CSV
df = pd.read_csv('ecommerce_estatistica.csv')

# Inicialização da aplicação Dash
app = dash.Dash(__name__)

# Layout da aplicação
app.layout = html.Div([
    html.H1("Dashboard de Estatísticas de E-commerce", style={'textAlign': 'center'}),
    
    # Gráfico de Dispersão: Preço vs. Nota
    html.Div([
        html.H3("Relação entre Preço e Nota"),
        dcc.Graph(
            figure=px.scatter(df, x='Preço', y='Nota', color='Marca', title="Preço vs Nota",
                               hover_data=['Título', 'N_Avaliações'])
        )
    ]),
    
    # Gráfico de Barras: Quantidade Vendida por Marca
    html.Div([
        html.H3("Quantidade Vendida por Marca"),
        dcc.Graph(
            figure=px.bar(df, x='Marca', y='Qtd_Vendidos', title="Quantidade Vendida por Marca",
                           color='Marca', text='Qtd_Vendidos')
        )
    ]),
    
    # Gráfico de Pizza: Frequência de Materiais
    html.Div([
        html.H3("Frequência de Materiais"),
        dcc.Graph(
            figure=px.pie(df, names='Material', values='Material_Freq', title="Frequência de Materiais")
        )
    ]),
    
    # Gráfico de Linha: Preço ao longo do tempo por temporada
    html.Div([
        html.H3("Variação de Preço por Temporada"),
        dcc.Graph(
            figure=px.line(df, x='Temporada', y='Preço', color='Marca', title="Preço ao longo do tempo por Temporada")
        )
    ])
])

# Execução da aplicação
if __name__ == '__main__':
    app.run_server(debug=True)

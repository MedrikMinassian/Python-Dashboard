# Python-Dashboard
Analytics Dashboard


pip install dash pandas plotly


import dash
import dash_core_components as dcc
import dash_html_components as html
from dash.dependencies import Input, Output
import pandas as pd
import plotly.express as px

# Sample data (replace this with your own dataset)
df = pd.DataFrame({
    'Category': ['A', 'B', 'C', 'D'],
    'Value': [4, 7, 1, 5]
})

# Initialize the Dash app
app = dash.Dash(__name__)

# Define the layout of the dashboard
app.layout = html.Div([
    html.H1("Analytics Dashboard"),
    
    # Dropdown for selecting a category
    dcc.Dropdown(
        id='category-dropdown',
        options=[
            {'label': category, 'value': category}
            for category in df['Category']
        ],
        value=df['Category'].iloc[0],
        multi=False
    ),
    
    # Bar chart to display values based on the selected category
    dcc.Graph(id='bar-chart'),

])

# Define callback to update the bar chart based on the selected category
@app.callback(
    Output('bar-chart', 'figure'),
    [Input('category-dropdown', 'value')]
)
def update_bar_chart(selected_category):
    filtered_df = df[df['Category'] == selected_category]
    fig = px.bar(filtered_df, x='Category', y='Value', title=f'Bar Chart for {selected_category}')
    return fig

# Run the app
if __name__ == '__main__':
    app.run_server(debug=True)


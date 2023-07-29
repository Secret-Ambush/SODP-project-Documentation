# Interface Building

## Overview

This documentation outlines the development of a Flight Capacity Prediction application using Streamlit. The application provides insights into flight capacity forecasting for a specific airline route and flight number. It offers two main functionalities: a Classification Model to predict whether a flight will be fully booked 30 days prior to its departure date and a Regression Model to predict the exact number of days before the flight is full.

The application leverages various components and caching mechanisms to ensure efficient data handling and a seamless user experience.

## Application Architecture

The Flight Capacity Prediction application is built using Streamlit, a Python library that simplifies creating web applications for data science and machine learning projects. Streamlit allows developers to turn data scripts into shareable web apps with minimal code, making it an excellent choice for this project.

## Streamlit Components

The application uses different Streamlit components to create a user-friendly interface and interact with the underlying data and models.

### Sidebar

The application incorporates a sidebar that acts as the primary control panel for the user. It features a title and an image, providing a professional and visually appealing look. Users can select between different functionalities using radio buttons:

- **Classification Model**: Enables users to explore the classification model for predicting full flights 30 days prior to departure.
- **Help: CM**: Offers documentation and guidance on using the Classification Model.
- **Regression Model**: Allows users to use the regression model to predict the exact number of days before a flight is fully booked.
- **Help: RM**: Provides documentation and explanations for the Regression Model.

### Classification Model Functionality

#### DATASET Section

In the Classification Model section, users are presented with an overview of the dataset. The application reads data from a Snowflake database using the `fetch_flight0001_data()` function. A loading spinner ensures a smooth experience while the data is being fetched.

The dataset is then displayed in a data table, allowing users to get a quick look at the available data. Additionally, users can trigger a Quick Analytics feature that generates and displays a flourish chart showcasing relevant data insights. The chart provides a visual representation of the dataset, helping users grasp patterns and trends effectively.

#### Classifier Model / Analytics Section

This section provides users with the tools to explore the Classifier Model and analyze flights based on specific criteria. Users can select the origin (lego), destination (legd), and flight number (fltnum) from drop-down menus.

Upon selecting these parameters, users can click the "Generate" button to see a summary of their choices. The application then processes the data and calculates the SODP (30 days prior) value. The SODP represents the number of days before the flight's departure when it is expected to be full. The result is displayed below the selection inputs, providing users with valuable insights.

### Regression Model Functionality

In the Regression Model section, users can predict the exact number of days before a flight is fully booked. The interface allows users to choose the origin (lego), destination (legd), flight number (fltnum), departure date, and days prior using dropdown menus and a slider.

The application then processes the selected data using the Regression Model and presents the results visually. Users can see the predicted SODP (days prior) on a calendar plot, which displays the days with the highest booking activity.

Furthermore, users can observe the error between the predicted and actual SODP values in a time series plot. This provides an understanding of the model's performance and accuracy in predicting flight capacity.

## Caching Mechanism

Streamlit offers caching mechanisms to enhance application performance by reducing redundant data processing. The application effectively utilizes caching for two critical functions:

1. **`@st.cache_resource`**: This decorator is applied to the `get_connection()` function, which creates a connection to the Snowflake database. By caching this resource, the application avoids establishing a new connection every time a user interacts with the dataset, optimizing data retrieval.

2. **`@st.cache_data`**: The `fetch_flight_data(flightnumber)` function is decorated with `@st.cache_data` to cache the data retrieved from the database. As data retrieval can be time-consuming, caching this resource ensures that data is fetched only once per user session, significantly improving response times.

## Conclusion

The Flight Capacity Prediction application built with Streamlit provides valuable insights into flight capacity forecasting using classification and regression models. By utilizing Streamlit's components and caching mechanisms, the application delivers a user-friendly experience with efficient data handling and visualization. The application's flexibility allows users to explore various flight routes and flight numbers, enabling data-driven decision-making for flight capacity management and optimization.

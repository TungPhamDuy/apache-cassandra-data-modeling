#   Apache Cassandra Data Modeling Project - Data Engineering

## Project Overview

This project focuses on designing and implementing an optimized data model for Apache Cassandra, a NoSQL column-family database, to support analytics for a music streaming application. The goal is to efficiently store and query user activity data (e.g., listening history, session details) that was previously stored in a messy CSV format.

This project was completed as part of the **Udacity Data Engineering with Microsoft Azure Nanodegree Program**.

## Problem Statement

A fictional music streaming company, "Sparkify," stores its user activity data in a single CSV file, which is inefficient for analytical queries. The task is to:
1.  **Extract:** Read raw event data from the CSV file.
2.  **Transform:** Process and clean the data.
3.  **Load:** Ingest the cleaned data into an Apache Cassandra database, ensuring tables are designed to answer specific analytical questions efficiently.

## Data Source

The dataset used in this project consists of event logs from a music streaming application, similar to the one provided by Udacity for the Data Engineering Nanodegree.
* **Original Data:** Each row in the CSV represents a single event (e.g., a song played).
* **Key Information:** Includes details about the user, session, song, artist, and timestamp.

## Project Structure

The repository is organized as follows:
.
├── cassandra_data_modeling.ipynb  # Main Jupyter Notebook containing the project code  
├── event_datafile_new.csv/        # Directory containing the raw event data (e.g., event_datafile_new.csv)  
├── images/                        # Directory for screenshots or diagrams  
├── README.md                      # This file  
└── requirements.txt               # (Optional) For listing Python dependencies  

## Key Technologies Used

* **Python 3:** The primary programming language for data processing and database interaction.
* **Apache Cassandra:** The NoSQL database for storing modeled data.
* **Pandas:** For data manipulation and analysis in Python.
* **Cassandra Driver for Python:** To connect and interact with Cassandra.
* **Jupyter Notebook:** For developing and showcasing the data modeling process.

## Data Modeling & Design Choices

The core of this project lies in designing Cassandra tables that are optimized for specific queries. Unlike relational databases, Cassandra favors denormalization and query-first design.

For this project, the following analytical questions were addressed, leading to the creation of separate tables:

1.  **`artist_song_by_session`:** Retrieve song information (artist, song title, song length) for a given session ID and item in session.
    * **Primary Key:** `(session_id, item_in_session)`
2.  **`user_songs_by_session`:** Retrieve details about the user, song, and session for songs played by a specific user in a specific session.
    * **Primary Key:** `(user_id, session_id, item_in_session)`
3.  **`song_listeners`:** Retrieve names of users who listened to a given song.
    * **Primary Key:** `(song_title, user_id)` (Clustering by `user_id` to ensure unique user entries and allow sorting).

Each table was carefully designed to ensure efficient lookups based on the anticipated query patterns, leveraging compound primary keys and clustering columns as needed.

## ETL Process

The ETL (Extract, Transform, Load) pipeline implemented in the Jupyter Notebook performs the following steps:

1.  **Extract:** Reads multiple CSV files from the `event_data` directory and concatenates them into a single Pandas DataFrame.
2.  **Transform:**
    * Handles missing values (if any).
    * Selects relevant columns.
    * Performs any necessary data type conversions.
3.  **Load:** Connects to the Apache Cassandra cluster, creates a keyspace and the designed tables, and then inserts the transformed data into the respective tables. Each insertion is optimized for Cassandra's write patterns.

## How to Run the Project

To run this project on your local machine:

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/](https://github.com/)[YOUR_GITHUB_USERNAME]/[YOUR_REPO_NAME].git
    cd [YOUR_REPO_NAME]
    ```
2.  **Install Dependencies:**
    It's recommended to use a virtual environment.
    ```bash
    pip install -r requirements.txt
    ```
    *(If `requirements.txt` is not provided, you'll need to install `pandas`, `cassandra-driver`, and `jupyter` manually).*
    ```bash
    pip install pandas cassandra-driver jupyter
    ```
3.  **Set up Apache Cassandra:**
    * Ensure you have an Apache Cassandra instance running. You can run it locally via Docker:
        ```bash
        docker run --name cassandra-dev -p 9042:9042 -d cassandra:latest
        ```
    * Verify Cassandra is running (e.g., `nodetool status` if you installed it directly, or `docker logs cassandra-dev` to check Docker container logs).
4.  **Run the Jupyter Notebook:**
    ```bash
    jupyter notebook cassandra_data_modeling.ipynb
    ```
5.  **Execute Cells:** Follow the steps in the notebook `cassandra_data_modeling.ipynb` to:
    * Connect to Cassandra.
    * Create the keyspace and tables.
    * Run the ETL process to load data.
    * Execute example queries to verify the data model.

## Results & Verification

The notebook includes sample queries executed against the populated Cassandra tables to demonstrate that the data model can efficiently answer the specified analytical questions. The output of these queries verifies the successful loading and retrieval of data as per the design.

## Future Improvements

* Implement more robust error handling in the ETL pipeline.
* Explore advanced Cassandra features like secondary indexes (if query patterns require it) or user-defined types (UDTs).
* Integrate with a data streaming platform (e.g., Apache Kafka) for real-time data ingestion.
* Deploy Cassandra on a cloud platform (e.g., Azure Kubernetes Service with Cassandra as a stateful set, or Azure Cosmos DB for Apache Cassandra API).

## Contributing

Feel free to fork this repository, submit pull requests, or open issues if you have suggestions or find bugs.

## License

This project is licensed under the [LICENSE_TYPE] License - see the `LICENSE` file for details.

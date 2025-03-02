# ScyllaDB Cloud Media Player Metrics - Rust

Project to store songs that you like to listen daily and keep track of them in a shape of a CLI!

![GYJ01DE-1.png](https://i.imgur.com/GYJ01DE.png)

## Prerequisites

* [Rust](https://www.rust-lang.org/tools/install)


## Running the project

Clone the repository into your machine:

```sh 
git clone https://github.com/scylladb/scylla-cloud-getting-started.git
cd scylla-cloud-getting-started/rust
```


Install the project dependencies and run the project: 

```sh
cargo run --  --username scylla --password your-cluster-password "node-0.aws-sa-east-1.1.clusters.scylla.cloud", "node-1.aws-sa-east-1.1.clusters.scylla.cloud", "node-2.aws-sa-east-1.1.clusters.scylla.cloud"
```

> Replace the variables with your cluster information


## Available Commands

Check which commands are currently available on this sample:

| Command  | Description |
|---|---|
| !new   | Add a new song to your liked songs list   |
| !delete  | Delete a specific song from your liked songs list   |
| !listen  | Creates a register of which song and when you listened to it  |
| !stress  | Retrieve all songs and create a 'stressing' loop to test a ScyllaDB Cloud Cluster |

## CQL Queries

All the CQL queries used on the project

```sql
CREATE KEYSPACE prod_media_player
    WITH replication = {'class': 'NetworkTopologyStrategy', 'replication_factor': '3'}
    AND durable_writes = true;

CREATE TABLE songs (
    id uuid,
    title text,
    album text,
    artist text,
    release_year int,
    created_at timestamp,
    PRIMARY KEY (id, created_at)
)

CREATE TABLE recently_played_songs (
    song_id uuid,
    listened_at timestamp,
    PRIMARY KEY (song_id, created_at)
)

CREATE TABLE played_songs_counter (
    song_id uuid,
    times_played count,
    PRIMARY KEY (song_id)
)

SELECT keyspace_name FROM system_schema.keyspaces WHERE keyspace_name = ?
SELECT keyspace_name,table_name FROM system_schema.tables WHERE keyspace_name = ? AND table_name = ?

SELECT * FROM songs
INSERT INTO recently_played_songs (song_id, listened_at) VALUES (?, ?)
UPDATE played_songs_counter SET times_played = times_played + 1 WHERE song_id = ?
DELETE FROM songs WHERE id = ?

```
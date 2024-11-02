input {
  beats {
    port => 5044
  }
}

filter {
  # Add any filters here
}

output {
  elasticsearch {
    hosts => ["http://192.168.56.111:9200"]
    index => "logstash-%{+YYYY.MM.dd}"
  }
}

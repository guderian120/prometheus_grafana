

```markdown
# Prometheus Alert Tracker with Grafana Visualization

![Grafana Dashboard Preview](/media/grafana_dashboard.gif)

This project creates a comprehensive system monitoring solution by combining Prometheus metrics collection with Grafana's powerful visualization capabilities. The system tracks vital server metrics, triggers alerts when thresholds are exceeded, and presents the data through interactive dashboards.

## Project Overview

I developed a Prometheus-based alert tracking system that exports system metrics to Grafana for creating complex, visually appealing visualizations. The solution combines:

- **Prometheus** for metrics collection and alert management
- **Python** with specialized libraries for metric exporting
- **Grafana** for advanced data visualization and dashboarding

## System Architecture

1. **Prometheus Server**: Collects and stores time-series metrics
2. **Python Exporter**: Custom exporter that gathers system metrics
3. **Grafana Server**: Visualizes data from Prometheus with interactive dashboards

## Implementation Details

### 1. Prometheus Installation

I installed Prometheus securely by creating a dedicated system user and group:

```bash
sudo useradd --no-create-home --shell /bin/false prometheus
sudo groupadd prometheus
sudo usermod -a -G prometheus prometheus
```

This security-focused approach ensures Prometheus runs with minimal privileges.

### 2. Python Metric Exporter

I developed a Python exporter (`python_exporter.py`) that uses:

- **psutil**: For gathering system metrics
- **prometheus-client**: For exposing metrics in Prometheus format

Key metrics collected:
- CPU usage (per core and aggregate)
- Disk I/O (read/write operations, bytes)
- Network traffic (inbound/outbound)
- Memory utilization (used, cached, available)
- System load averages

Example exporter code snippet:
```python
from prometheus_client import start_http_server, Gauge
import psutil

cpu_usage = Gauge('system_cpu_percent', 'Current CPU usage percent')
mem_usage = Gauge('system_memory_percent', 'Current memory usage percent')

def collect_metrics():
    cpu_usage.set(psutil.cpu_percent())
    mem_usage.set(psutil.virtual_memory().percent)
```

### 3. Prometheus Basic Visualization

While Prometheus provides basic graphing capabilities, its visualization options are limited:

![Prometheus Basic Graph](/media/prometheus_graph.png)

*The native Prometheus graph shows raw metrics but lacks advanced visualization features*

### 4. Grafana Integration

I configured Grafana to address Prometheus' visualization limitations by:

1. **Connecting to Prometheus Data Source**  
   ![Grafana Data Source Setup](/media/grafana_prometheus_data_source_setup.png)

2. **Creating Interactive Dashboards**  
   - Developed comprehensive dashboards with multiple visualization types
   - Implemented templating for flexible metric selection
   - Added annotation support for event tracking

3. **Setting Up Alerting**  
   - Configured threshold-based alerts
   - Integrated with notification channels (Email, Slack, etc.)
   - Implemented alert escalation policies

## Key Features

- **Real-time System Monitoring**: Track all critical server metrics
- **Beautiful Visualizations**: Grafana's rich visualization library
- **Custom Alerting**: Get notified when metrics exceed thresholds
- **Historical Analysis**: Compare current performance with historical trends
- **Exportable Dashboards**: Share dashboards with team members

## Media Gallery

1. Prometheus Basic Graph  
   ![Prometheus Graph](/media/prometheus_graph.png)

2. Grafana Data Source Configuration  
   ![Grafana Data Source Setup](/media/grafana_prometheus_data_source_setup.png)

3. Grafana Dashboard Demo  
   ![Grafana Dashboard](/media/grafana_dashboard.gif)

## Getting Started

### Prerequisites
- Linux server (tested on Ubuntu 20.04)
- Python 3.8+
- Docker (for Grafana installation)

### Installation Steps
1. Clone this repository
2. Install Prometheus following the [official guide](https://prometheus.io/docs/prometheus/latest/installation/)
3. Set up the Python exporter:
   ```bash
   pip install -r requirements.txt
   python python_exporter.py
   ```
4. Install Grafana:
   ```bash
   docker run -d -p 3000:3000 --name=grafana grafana/grafana-enterprise
   ```

## Configuration

1. Configure Prometheus to scrape your Python exporter by editing `prometheus.yml`:
   ```yaml
   scrape_configs:
     - job_name: 'python_exporter'
       static_configs:
         - targets: ['localhost:8000']
   ```

2. Set up Grafana:
   - Login to `http://localhost:3000` (admin/admin)
   - Add Prometheus as data source (URL: `http://prometheus:9090`)
   - Import the sample dashboard from `dashboards/sample.json`

## Alert Configuration

To configure alerts in Grafana:
1. Navigate to Alert -> Notification channels
2. Add your preferred channel (Email, Slack, etc.)
3. Create alert rules in your dashboard panels

Example CPU alert rule:
```
avg_over_time(system_cpu_percent{instance="localhost:8000"}[5m]) > 80
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Prometheus team for the excellent monitoring system
- Grafana Labs for their visualization tools
- psutil developers for the comprehensive system metrics library
```


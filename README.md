# MediaMTX Grafana Dashboard

Comprehensive Grafana dashboard for monitoring [MediaMTX](https://github.com/bluenviron/mediamtx) media server with 100% metric coverage.

![MediaMTX — CameraCloud NVR V3](images/MediaMTX — CameraCloud NVR V3.png)


## Overview

This dashboard provides real-time monitoring and visualization of all MediaMTX metrics including:

- **Server Status**: Health check and uptime monitoring
- **Paths**: Active streams, bandwidth (ingest/output), and reader statistics
- **RTSP/RTSPS**: Connections, sessions, RTP/RTCP packet statistics
- **WebRTC**: Session management, RTP quality metrics (packets, jitter, loss)
- **HLS**: Muxer count and bandwidth tracking
- **RTMP/RTMPS**: Connection count and bandwidth metrics
- **SRT**: Advanced metrics including retransmissions, loss, RTT, buffer usage

## Features

✅ **Complete metric coverage** - All MediaMTX metrics from the [official documentation](https://mediamtx.org/docs/usage/metrics)  
✅ **Protocol-specific sections** - Organized by streaming protocol  
✅ **Quality metrics** - RTP/RTCP packet loss, jitter, errors for RTSP/RTSPS and WebRTC  
✅ **Performance insights** - SRT retransmissions, RTT, send/receive rates, buffer statistics  
✅ **Bandwidth monitoring** - Per-path and total bandwidth visualization  
✅ **Connection tracking** - Active sessions and connections per protocol  

## Prerequisites

- **Grafana** 8.0 or higher
- **Prometheus** configured to scrape MediaMTX metrics
- **MediaMTX** with metrics enabled

## Installation

### 1. Enable MediaMTX Metrics

Edit your MediaMTX configuration file:

```yaml
metrics: yes
metricsAddress: :9998
```

Restart MediaMTX and verify metrics are available:

```bash
curl http://localhost:9998/metrics
```

### 2. Configure Prometheus

Add MediaMTX as a scrape target in your `prometheus.yml`:

```yaml
scrape_configs:
  - job_name: 'mediamtx'
    static_configs:
      - targets: ['localhost:9998']
```

### 3. Import Dashboard to Grafana

**Option A: Import via UI**

1. Open Grafana web interface
2. Go to **Dashboards** → **Import**
3. Upload `mediamtx-dashboard.json` or paste its content
4. Select your Prometheus datasource
5. Click **Import**

**Option B: Import via API**

```bash
curl -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d @mediamtx-dashboard.json \
  http://your-grafana-url/api/dashboards/db
```

**Option C: Provisioning**

Place the dashboard in your Grafana provisioning directory:

```bash
cp mediamtx-dashboard.json /etc/grafana/provisioning/dashboards/
```

## Dashboard Sections

### Overview
- MediaMTX status (up/down)
- Active paths count
- Session counts by protocol (RTSP, WebRTC, HLS, RTMP)

### Paths — Bandwidth & Readers
- Per-path ingest bitrate (received)
- Per-path output bitrate (sent)
- Readers per path
- Total bandwidth (all paths)

### RTSP Sessions (Expanded)
- RTP packets (received/sent/lost)
- RTP jitter and errors
- RTCP packets (received/sent/errors)
- Session bytes transferred

### RTSP Connections
- Connection count
- Bytes received/sent per connection

### RTSPS (Secure RTSP)
- Complete secure RTSP monitoring
- All metrics from RTSP section for encrypted connections

### WebRTC Sessions (Expanded)
- Session count and state
- Bytes transferred
- RTP packet statistics (received/sent/lost)
- Jitter and error tracking

### HLS Streaming (Expanded)
- Active muxer count
- Bytes sent per muxer

### RTMP/RTMPS Connections (Expanded)
- Connection count
- Bandwidth per connection

### SRT Connections (Complete)
- Connection state tracking
- Bytes and packets (sent/received/retransmitted/lost)
- Performance metrics (send/receive rates, RTT)
- Buffer utilization (send/receive buffers)
- Link capacity and quality indicators

## Variables

The dashboard includes variables for filtering:

- **datasource**: Select Prometheus datasource
- **job**: Filter by Prometheus job name
- **path**: Filter by MediaMTX path name

## Metrics Reference

For detailed information about MediaMTX metrics, refer to:
- [Official MediaMTX Metrics Documentation](https://mediamtx.org/docs/usage/metrics)

### Metric Categories

| Category | Metric Prefix | Description |
|----------|--------------|-------------|
| Server | `up` | Server health status |
| Paths | `paths*` | Stream paths, bandwidth, readers |
| RTSP | `rtsp_conns*`, `rtsp_sessions*` | RTSP connections and sessions |
| RTSPS | `rtsps_conns*`, `rtsps_sessions*` | Secure RTSP connections and sessions |
| RTMP | `rtmp_conns*` | RTMP/RTMPS connections |
| WebRTC | `webrtc_sessions*` | WebRTC sessions and quality |
| HLS | `hls_muxers*` | HLS streaming muxers |
| SRT | `srt_conns*` | SRT connections with advanced metrics |

## Customization

The dashboard is fully customizable. Common modifications:

- **Thresholds**: Adjust color thresholds for alerts (e.g., packet loss > 5%)
- **Refresh rate**: Change auto-refresh interval (default: 30s)
- **Time range**: Modify default time window
- **Panels**: Add/remove panels based on your needs
- **Alerting**: Configure Grafana alerts on critical metrics

## Troubleshooting

### No data in dashboard

1. Verify MediaMTX metrics are enabled: `curl http://localhost:9998/metrics`
2. Check Prometheus is scraping: `http://prometheus:9090/targets`
3. Verify datasource in Grafana is configured correctly
4. Check variable `job` matches your Prometheus job name

### Missing metrics

- Ensure you're running a recent version of MediaMTX
- Some metrics only appear when relevant connections exist (e.g., WebRTC metrics when clients are connected)

### High cardinality warnings

If you have many paths/sessions, consider:
- Increasing Prometheus memory
- Using recording rules for frequently-queried metrics
- Filtering variables to specific paths of interest

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## License

This dashboard configuration is provided as-is for use with MediaMTX.

## About

**Grafana Dashboard for MediaMTX Media Server**

This dashboard provides comprehensive monitoring and visualization of all MediaMTX streaming server metrics. It covers all major streaming protocols (RTSP, WebRTC, HLS, RTMP, SRT) with detailed quality and performance metrics.

MediaMTX is a modern, powerful, and easy-to-use media server that supports multiple streaming protocols. This dashboard helps you monitor server health, track stream quality, analyze bandwidth usage, and diagnose issues in real-time.

**Topics**: grafana, dashboard, mediamtx, monitoring, prometheus, rtsp, webrtc, hls, rtmp, srt, streaming, metrics, media-server

---

**Repository maintained by the community for MediaMTX users.**

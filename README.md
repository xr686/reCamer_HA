# reCamer_HA
You can connect multiple reCamera to HomeAssistant, and after connecting to HA, you can see the video stream from the reCamera, as well as the detection results and quantities.
您可以将多个 reCamera 连接到 HomeAssistant，连接到 HA 后，您可以查看来自 reCamera 的视频流以及检测结果和数量。


## 📊 Dashboard Configuration

To display the RTSP live stream and real-time YOLO detection details (labels & counts) simultaneously, you can use a **Vertical Stack** card.

1. Go to your Home Assistant Dashboard -> **Edit Dashboard**.
2. Click **Add Card** -> Search for **"Manual"**.
3. Copy and paste the YAML code below.
4. **IMPORTANT:** Replace `YOUR_IP_SUFFIX` with your actual device IP suffix (e.g., `192_168_1_50` or `2` depending on how HA named your entity). Check **Developer Tools -> States** to find your correct Entity IDs.

```yaml
type: vertical-stack
cards:
  # 1. Live Stream Card
  - type: picture-entity
    # CHANGE THIS ID to your camera entity
    entity: camera.recamera_stream_YOUR_IP_SUFFIX
    name: reCamera Live
    show_state: false
    camera_view: live
    # aspect_ratio: '16:9' # Uncomment if needed

  # 2. AI Statistics Card
  - type: markdown
    content: >
      ## 📊 Detection Details
      
      {# CHANGE THIS ID to your sensor entity #}
      **Total Objects:** {{ states('sensor.recamera_detection_YOUR_IP_SUFFIX') }}
      
      ---
      
      {# CHANGE THIS ID to your sensor entity (Must match above) #}
      {% set all_attrs = states.sensor.recamera_detection_YOUR_IP_SUFFIX.attributes %}
      
      {% for key, value in all_attrs.items() %}
        {# Filter out internal attributes, show only YOLO labels #}
        {% if key not in ['friendly_name', 'icon', 'total', 'timestamp', 'device_class', 'payload', 'stream_source'] %}
        - **{{ key }}:** {{ value }}
        {% endif %}
      {% endfor %}

```


## 📊 仪表盘配置 (Dashboard Setup)

要同时显示实时视频流和 AI 检测详情（YOLO 标签及数量），请在 Home Assistant 仪表盘中使用 **垂直堆叠 (Vertical Stack)** 卡片。

1. 进入仪表盘 -> 点击右上角 **编辑仪表盘**。
2. 点击右下角 **添加卡片** -> 搜索并选择 **"手动 (Manual)"**。
3. 复制以下 YAML 代码并粘贴。
4. **注意：** 请务必将代码中的 `YOUR_IP_SUFFIX` 替换为你实际的设备 IP 后缀（例如 `192_168_42_1`）。你可以在 **开发者工具 -> 状态** 中找到准确的实体 ID。

```yaml   
type: vertical-stack   类型:垂直叠加
cards:   卡:
  # 1. 视频流卡片
  - type: picture-entity   —类型：picture-entity
    # 请修改这里的实体 ID
    entity: camera.recamera_stream_YOUR_IP_SUFFIX实体:camera.recamera_stream_YOUR_IP_SUFFIX
    name: reCamera 实时监控
    show_state: false   show_state:假
    camera_view: live   camera_view:生活

  # 2. 数据统计卡片
  - type: markdown   -类型：markdown
    content: >   内容:在
      ## 📊 检测详情
      
      {# 请修改这里的实体 ID #}
      **总数:** {{ states('sensor.recamera_detection_YOUR_IP_SUFFIX') }}
      
      ---
      
      {# 请修改这里的实体 ID (必须与上面一致) #}
      {% set all_attrs = states.sensor.recamera_detection_YOUR_IP_SUFFIX.attributes %}{% set all_attrs = states.sensor.recamera_detection_YOUR_IP_SUFFIX。属性%}
      
      {% for key, value in all_attrs.items() %}{%表示键，all_attrs中的值项目()%}
        {# 排除系统属性，只显示检测到的物体 #}
        {% if key not in ['friendly_name', 'icon', 'total', 'timestamp', 'device_class', 'payload'] %}
        - **{{ key }}:** {{ value }}
        {% endif %}
      {% endfor %}
```

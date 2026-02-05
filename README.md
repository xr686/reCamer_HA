# reCamer_HA
You can connect multiple reCamera to HomeAssistant, and after connecting to HA, you can see the video stream from the reCamera, as well as the detection results and quantities.
您可以将多个 reCamera 连接到 HomeAssistant，连接到 HA 后，您可以查看来自 reCamera 的视频流以及检测结果和数量。


## 📊 Dashboard Configuration📊Dashboard配置

To display the RTSP live stream and real-time YOLO detection details (labels & counts) simultaneously, you can use a **Vertical Stack** card.

1. Go to the dashboard -> Click on **Edit Dashboard** in the top right corner.
2. Click on **Add Card** in the bottom right corner -> Search and select **"Manual"**.
3. Copy the following YAML code and paste it.
4. **Important:** Please replace `YOUR_IP_SUFFIX` in the code with your actual device IP suffix (e.g., `camera.recamera_stream_3`). You can find this information here: HA Panel – Settings – Devices & Services – reCamera AI – x entities – Identifier for the entity corresponding to the reCamera Stream/Detection.

```yaml  
type: vertical-stack
cards:
  # 1. Video stream card
  - type: picture-entity
    # Fill in the physical ID of your reCamera Stream here, and fill in the physical ID of reCamera Detection in the other two places.
    entity: YOUR_IP_SUFFIX
    name: reCamera LIVE
    show_state: false
    camera_view: live

  # 2. Data statistics card
  - type: markdown
    content: >
      ## 📊 Detection details
      
      {# Get the total number #}
      **Total:** {{ states('YOUR_IP_SUFFIX') }}
      
      ---
      
      {# Obtain detailed attributes #}
      {% set all_attrs = states.YOUR_IP_SUFFIX.attributes %}
      
      {% for key, value in all_attrs.items() %}
        {# Exclude system properties #}
        {% if key not in ['friendly_name', 'icon', 'total', 'timestamp', 'device_class', 'payload'] %}
        - **{{ key }}:** {{ value }}
        {% endif %}
      {% endfor %}

```


## 📊 仪表盘配置 (Dashboard Setup)

要同时显示实时视频流和 AI 检测详情（YOLO 标签及数量），请在 Home Assistant 仪表盘中使用 **垂直堆叠 (Vertical Stack)** 卡片。

1. 进入仪表盘 -> 点击右上角 **编辑仪表盘**。
2. 点击右下角 **添加卡片** -> 搜索并选择 **"手动 (Manual)"**。
3. 复制以下 YAML 代码并粘贴。
4. **注意：** 请务必将代码中的 `YOUR_IP_SUFFIX` 替换为你实际的设备 IP 后缀（例如 `camera.recamera_stream_3`）。你可以在这里找到：HA面板——设置——设备与服务——reCamera AI——x个实体——对应的reCamera Stream/Detection后面的实体标识符

```yaml   
type: vertical-stack
cards:
  # 1. 视频流卡片
  - type: picture-entity
    # 这里只填你的reCamera Stream的实体ID，剩下两处填写reCamera Detection的实体ID
    entity: YOUR_IP_SUFFIX
    name: reCamera 实时监控
    show_state: false
    camera_view: live

  # 2. 数据统计卡片
  - type: markdown
    content: >
      ## 📊 检测详情
      
      {# 获取总数 #}
      **总数:** {{ states('YOUR_IP_SUFFIX') }}
      
      ---
      
      {# 获取详细属性 #}
      {% set all_attrs = states.YOUR_IP_SUFFIX.attributes %}
      
      {% for key, value in all_attrs.items() %}
        {# 排除系统属性 #}
        {% if key not in ['friendly_name', 'icon', 'total', 'timestamp', 'device_class', 'payload'] %}
        - **{{ key }}:** {{ value }}
        {% endif %}
      {% endfor %}
```

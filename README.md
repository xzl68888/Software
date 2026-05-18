# ========================================================
# ⚡ 金沙国际娱乐 (JINSHA INTERNATIONAL) 生产环境核心配置
# ========================================================
NODE_ENV=production

# 1. 基础设施密码
DATABASE_URL=postgres://jinsha_admin:JinshaSec_2026_Prod@postgres-db:5432/jinsha_vipsys
REDIS_URL=redis://redis-cache:6379

# 2. 核心品牌安全加固密钥
JWT_SECRET=JinshaInternational_Sec_Jwt_2026_VipString
COOKIE_SECRET=JinshaInternational_Cookie_Glob_Secret

# 3. 三方游戏厂商上游网关
GAME_API_URL=https://api.thirdpartygame.com
GAME_MERCHANT_CODE=JINSHA_INTERNATIONAL_PROD
SABA_SPORTS_API=https://api.sabasports.com/v1
LOTTERY_CORE_URL=https://api.officiallottery.com

limit_req_zone $binary_remote_addr zone=jinsha_limit:10m rate=10r/s;

server {
    listen 80;
    server_name localhost;

    # 隐藏 Nginx 版本号，全面隐藏源站特征
    server_tokens off;

    # 品牌化专属安全响应头
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header X-Powered-By "Jinsha-Security-Core" always; # 自定义品牌安全防护标签

    # 1. 金沙国际娱乐 - 玩家客户端大厅
    location / {
        root /usr/share/nginx/html/user;
        index index.html;
        try_files $uri $uri/ /index.html;
    }

    # 2. 金沙国际娱乐 - 顶权风控控制中心
    location /admin {
        alias /usr/share/nginx/html/admin;
        index index.html;
        try_files $uri $uri/ /admin/index.html;
    }

    # 3. 核心 API 路由与限流
    location /api/ {
        limit_req zone=jinsha_limit burst=5 nodelay;

        proxy_pass http://backend-server:3001;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
} 

import React, { useEffect, useState } from 'react';
import { Card, Col, Row, Tag, Button, Modal, message, Badge } from 'antd';
import { GoldOutlined, TrophyFilled, SafetyCertificateOutlined } from '@ant-design/icons';
import axios from 'axios';

interface GameItem {
  provider: string;
  game_name: string;
  game_type: string;
  status: number;
}

export const JinshaGameLobby: React.FC = () => {
  const [games, setGames] = useState<GameItem[]>([]);
  const [loading, setLoading] = useState(false);

  const fetchGames = async () => {
    try {
      const token = localStorage.getItem('user_token');
      const res = await axios.get('http://localhost:3001/api/games/list', {
        headers: { Authorization: `Bearer ${token}` }
      });
      if (res.data.success) {
        setGames(res.data.data);
      }
    } catch (err) {
      message.error('拉取金沙娱乐大厅列表失败');
    }
  };

  useEffect(() => {
    fetchGames();
  }, []);

  return (
    <div style={{ padding: '40px', background: '#0a0a0a', minHeight: '100vh' }}>
      {/* 金沙国际黑金奢华头部 */}
      <div style={{ borderBottom: '2px solid #dfb76c', paddingBottom: '20px', marginBottom: '40px' }}>
        <h1 style={{ color: '#dfb76c', fontSize: '32px', fontWeight: 'bold', letterSpacing: '2px', display: 'flex', alignItems: 'center' }}>
          <GoldOutlined style={{ marginRight: '15px', color: '#dfb76c' }} /> 
          金沙国际娱乐 | JINSHA INTERNATIONAL ENTERPRISE
        </h1>
        <p style={{ color: '#8c8c8c', margin: '5px 0 0 45px' }}>
          <SafetyCertificateOutlined style={{ color: '#52c41a' }} /> 银行级安全防护已开启 • 实时对账无感结算中心
        </p>
      </div>
      
      {/* 5 大定制娱乐品类矩阵 */}
      <Row gutter={[24, 24]}>
        {games.map((game) => (
          <Col span={8} key={game.provider}>
            <Badge.Ribbon 
              text={game.status === 1 ? "⚡ 极速连通" : "⚙️ 品牌维护"} 
              color={game.status === 1 ? "#dfb76c" : "#ff4d4f"}
            >
              <Card
                hoverable={game.status === 1}
                style={{ 
                  background: '#141414', 
                  border: '1px solid #303030',
                  borderRadius: '8px',
                  boxShadow: '0 4px 12px rgba(0,0,0,0.5)'
                }}
                actions={[
                  <Button 
                    type="primary" 
                    style={{ background: game.status === 1 ? '#dfb76c' : '#333', borderColor: game.status === 1 ? '#dfb76c' : '#333', color: '#000', fontWeight: 'bold' }}
                    disabled={game.status === 0}
                    loading={loading}
                    onClick={() => {/* 唤起安全 Iframe 局内运行机制 */}}
                  >
                    {game.status === 1 ? "一键登入场局" : "线路技术优化"}
                  </Button>
                ]}
              >
                <Card.Meta
                  title={<span style={{ color: '#fff', fontSize: '18px' }}>{game.game_name}</span>}
                  description={
                    <div style={{ marginTop: '10px' }}>
                      <p style={{ color: '#666', fontSize: '12px', marginBottom: '8px' }}>专属定制上游：{game.provider} Secure Link</p>
                      <Tag color="#dfb76c" style={{ color: '#000', fontWeight: 'bold' }}>VIP 特权级盘口</Tag>
                    </div>
                  }
                />
              </Card>

              import React, { useState } from 'react';
import { FloatButton, Modal, Button, List, Typography } from 'antd';
import { CustomerServiceOutlined, MessageOutlined, CommentOutlined, ShieldOutlined } from '@ant-design/icons';

const { Text } = Typography;

export const JinshaCustomerService: React.FC = () => {
  const [isModalOpen, setIsModalOpen] = useState(false);

  // ⚡ 金沙官方客服配置：配置你的机器人和官方链接
  const serviceChannels = [
    {
      name: 'Telegram 24小时 VIP 官服',
      icon: <MessageOutlined style={{ color: '#1890ff', fontSize: 22 }} />,
      desc: '支持大额充提、VIP特权申请、秒级响应',
      // t.me/share/url 或直接跳转可以完美唤起本地 APP
      actionUrl: 'https://t.me/XuanGuang_bot', 
      btnText: '一键唤起 Telegram',
      color: '#1890ff'
    },
    {
      name: 'LINE 专属快捷客服',
      icon: <CommentOutlined style={{ color: '#52c41a', fontSize: 22 }} />,
      desc: '专线专属接待，技术与账务一站式服务',
      // LINE 唤起协议
      actionUrl: 'https://line.me/ti/p/~jinsha_service', 
      btnText: '一键连接 LINE',
      color: '#52c41a'
    }
  ];

  const handleOpenChannel = (url: string) => {
    // 开启新窗口，并使用 noopener/noreferrer 隐藏玩家来源网址，保护平台域名安全
    window.open(url, '_blank', 'noopener,noreferrer');
  };

  return (
    <>
      {/* 右下角常驻的金沙专属客服悬浮按钮 */}
      <FloatButton
        icon={<CustomerServiceOutlined />}
        type="primary"
        style={{ 
          right: 30, 
          bottom: 30, 
          width: 60, 
          height: 60, 
          backgroundColor: '#dfb76c', 
          borderColor: '#dfb76c' 
        }}
        tooltip={<div>金沙国际 | 联络官服</div>}
        onClick={() => setIsModalOpen(true)}
      />

      {/* 客服通道选择沙盒视窗 */}
      <Modal
        title={
          <div style={{ color: '#dfb76c', fontSize: 18, fontWeight: 'bold' }}>
            <ShieldOutlined /> 金沙国际娱乐 - 官方安全通道
          </div>
        }
        open={isModalOpen}
        onCancel={() => setIsModalOpen(false)}
        footer={null}
        centered
        width={450}
        bodyStyle={{ background: '#141414', padding: '10px 0' }}
        style={{ content: { background: '#141414', border: '1px solid #dfb76c' } }}
      >
        <div style={{ padding: '0 20px 15px 20px', color: '#8c8c8c', fontSize: 12 }}>
          ⚠️ 提示：为保障您的资金与个人隐私安全，金沙国际官方客服绝不会在聊天中向您索要登录密码。请认准下方安全认证通道。
        </div>

        <List
          itemLayout="horizontal"
          dataSource={serviceChannels}
          renderItem={(channel) => (
            <List.Item
              style={{ 
                padding: '20px', 
                borderBottom: '1px solid #222',
                display: 'flex',
                justifyContent: 'space-between',
                alignItems: 'center'
              }}
            >
              <List.Item.Meta
                avatar={channel.icon}
                title={<span style={{ color: '#fff', fontWeight: 'bold', fontSize: 15 }}>{channel.name}</span>}
                description={<span style={{ color: '#666', fontSize: 12 }}>{channel.desc}</span>}
              />
              <Button
                type="primary"
                style={{ 
                  backgroundColor: channel.color, 
                  borderColor: channel.color,
                  borderRadius: '4px',
                  fontWeight: 'bold',
                  fontSize: 12
                }}
                onClick={() => handleOpenChannel(channel.actionUrl)}
              >
                {channel.btnText}
              </Button>
            </List.Item>
          )}
        />
        
        <div style={{ textAlign: 'center', marginTop: 20, paddingBottom: 10 }}>
          <Text type="secondary" style={{ color: '#434343', fontSize: 11 }}>
            JINSHA INTERNATIONAL SECURE DISPATCH SYSTEM © 2026
          </Text>
        </div>
      </Modal>
    </>
  );
};

frontend-web:
    build:
      context: .
      target: frontend
    container_name: xigang-frontend
    ports:
      - "8080:80"
    volumes:
      # ⚡ 核心：将本地的高防 index.html 实时映射到 Nginx 根目录，方便秒级调试
      - ./index.html:/usr/share/nginx/html/index.html
    depends_on:
      - backend-server
    networks:
      - xigang-network
      

            </Badge.Ribbon>
          </Col>
        ))}
      </Row>
    </div>
  );
};
 
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, shrink-to-fit=no">
    
    <meta name="referrer" content="no-referrer">
    
    <title>金沙国际娱乐 | 官方安全接入中心</title>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tailwindcss/2.2.19/tailwind.min.js"></script>
    
    <style>
        body { background-color: #0a0a0a; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; }
        .gold-text { color: #dfb76c; }
        .gold-bg { background-color: #dfb76c; }
        .gold-border { border-color: #dfb76c; }
        .glass-card { background: rgba(20, 20, 20, 0.85); border: 1px solid rgba(223, 183, 108, 0.15); box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.5); }
        .active-btn { transition: all 0.2s ease-in-out; }
        .active-btn:active { transform: scale(0.95); opacity: 0.9; }
    </style>
</head>
<body class="flex flex-col min-h-screen text-gray-300 antialiased select-none">
    
    <div class="w-full text-center py-2 bg-black border-b border-gray-900 text-xs tracking-widest gold-text">
        🔒 银行级全渠道安全加密通道已成功挂载
    </div>

    <main class="flex-grow flex flex-col justify-center items-center px-5 py-8 max-w-md mx-auto w-full">
        
        <div class="text-center mb-8">
            <h1 class="text-3xl font-extrabold tracking-wider gold-text mb-2">金沙国际娱乐</h1>
            <p class="text-xs tracking-widest text-gray-500 uppercase">JINSHA INTERNATIONAL ENTERTAINMENT</p>
            <div class="w-16 h-0.5 gold-bg mx-auto mt-4 rounded"></div>
        </div>

        <div class="w-full glass-card rounded-xl p-5 mb-6 text-center">
            <p class="text-sm text-gray-400 leading-relaxed">
                欢迎来自 <span class="gold-text font-bold">Telegram</span> 的特邀贵宾。<br>
                系统已为您阻断黑客劫持，请选择官方绿色通道切入金沙内部盘口。
            </p>
        </div>

        <div class="w-full space-y-4">
            
            <button onclick="secureRedirect('https://wxaurl.cn/t/你的微信小程序短链')" 
                    class="active-btn w-full py-4 rounded-xl font-bold text-center text-black gold-bg text-base shadow-lg flex items-center justify-center space-x-2">
                <span>🚀 打开官方小程序大厅</span>
            </button>

            <button onclick="secureRedirect('https://your-real-lobby.com/?auth_token=secure_dispatch')" 
                    class="active-btn w-full py-4 rounded-xl font-bold text-center bg-transparent gold-text gold-border border text-base flex items-center justify-center space-x-2">
                <span>🎰 进入网页综合大厅 (H5端)</span>
            </button>

            <button onclick="secureRedirect('https://t.me/+qMn7JJo6Ds83YWQ1')" 
                    class="active-btn w-full py-4 rounded-xl font-bold text-center bg-gray-900 text-gray-300 text-sm flex items-center justify-center space-x-2 border border-gray-800">
                <span>💬 申请加入 VIP 官方特邀群</span>
            </button>
            
        </div>

    </main>

    <footer class="w-full text-center py-6 text-gray-600 text-xs tracking-wider">
        <p>JINSHA SECURE GATEWAY PROTOCOL v2.9</p>
        <p class="mt-1" style="font-size: 10px;">防篡改、防监听、防劫持天网系统安全托管中</p>
    </footer>

    <script {nonce}>
        function secureRedirect(targetUrl) {
            console.log("🔒 [天网风控] 正在执行防追踪安全重定向，源端 Referrer 已彻底截断...");
            
            // 建立隐形防追踪硬隔离链接
            const secureAnchor = document.createElement('a');
            secureAnchor.href = targetUrl;
            secureAnchor.rel = 'noopener noreferrer';
            secureAnchor.target = '_blank';
            
            // 触发点击，实现无感跳转
            document.body.appendChild(secureAnchor);
            secureAnchor.click();
            document.body.removeChild(secureAnchor);
        }

        // 手机端防截图/防恶意审查元素微加固
        document.addEventListener('contextmenu', e => e.preventDefault());
    </script>
</body>
</html>

 

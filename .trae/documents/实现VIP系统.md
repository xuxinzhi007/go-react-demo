# 实现VIP系统

## 设计方案
在现有User模型中添加VIP相关字段，包括：
- `IsVip`：是否开通VIP
- `VipStartAt`：VIP开通时间
- `VipEndAt`：VIP到期时间

## 实现步骤

### 1. 修改User模型
- 在`backend/model/user.go`中添加VIP相关字段
- 确保字段类型和gorm标签正确

### 2. 更新用户服务层
- 在`backend/service/user_service.go`中添加VIP相关方法：
  - `UpdateUserVip`：更新用户VIP信息
  - `GetUserVipStatus`：获取用户VIP状态
  - `CheckUserVip`：检查用户是否为有效VIP

### 3. 更新用户控制器
- 在`backend/api/user_controller.go`中添加VIP相关API：
  - `POST /api/user/:id/vip`：开通/更新VIP
  - `GET /api/user/:id/vip`：获取VIP信息
  - `GET /api/user/:id/vip/check`：检查是否为有效VIP

### 4. 自动迁移模型
- 重启服务器时，GORM会自动迁移模型，添加新的VIP字段到数据库表

## API接口设计

| 方法 | 路径 | 功能 |
|------|------|------|
| POST | /api/user/:id/vip | 开通/更新用户VIP |
| GET | /api/user/:id/vip | 获取用户VIP信息 |
| GET | /api/user/:id/vip/check | 检查用户是否为有效VIP |

## 请求/响应示例

### 开通VIP请求
```json
{
  "is_vip": true,
  "vip_start_at": "2025-12-17T00:00:00Z",
  "vip_end_at": "2026-12-17T00:00:00Z"
}
```

### 响应示例
```json
{
  "code": 200,
  "message": "VIP更新成功",
  "data": {
    "id": 1,
    "username": "test",
    "email": "test@example.com",
    "is_vip": true,
    "vip_start_at": "2025-12-17T00:00:00Z",
    "vip_end_at": "2026-12-17T00:00:00Z",
    "created_at": "2025-12-17T00:00:00Z",
    "updated_at": "2025-12-17T00:00:00Z"
  }
}
```

## 实现注意事项
1. 确保VIP时间字段使用正确的time.Time类型
2. 添加适当的gorm标签，确保数据库字段类型正确
3. 实现自动检查VIP状态的逻辑（到期自动失效）
4. 确保API接口的安全性，添加适当的权限验证

## 后续扩展建议
1. 可以添加VIP等级字段，支持不同等级的VIP
2. 可以添加VIP开通记录，记录每次开通的详情
3. 可以添加自动续费功能
4. 可以添加VIP权限验证中间件

现在我将开始实现这个VIP系统。
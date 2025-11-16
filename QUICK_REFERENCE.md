# Urban Things API - Quick Reference

## 🚀 Quick Start

### 1. Register
```bash
POST /api/v2/admin/register
{
  "name": "Your Name",
  "email": "your@email.com",
  "password": "password123",
  "password_confirmation": "password123"
}
```

### 2. Login
```bash
POST /api/v2/admin/login
{
  "email": "your@email.com",
  "password": "password123"
}
# Returns: { "token": "...", "user": {...} }
```

### 3. Create Tenant
```bash
POST /api/v2/admin/tenants
Authorization: Bearer YOUR_TOKEN
{
  "name": "My Store"
}
```

### 4. Use API
```bash
GET /api/v2/admin/products
Authorization: Bearer YOUR_TOKEN
X-Tenant-ID: 123
```

## 📋 All Endpoints

### Authentication
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/v2/admin/register` | Register account |
| POST | `/api/v2/admin/login` | Login |
| GET | `/api/v2/admin/who_am_i` | Get user info |
| POST | `/api/v2/admin/logout` | Logout |

### Tenants
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v2/admin/tenants` | List tenants |
| POST | `/api/v2/admin/tenants` | Create tenant |
| GET | `/api/v2/admin/tenants/{id}` | Get tenant |
| POST | `/api/v2/admin/tenants/{id}` | Update tenant |
| DELETE | `/api/v2/admin/tenants/{id}` | Delete tenant |

### Team Members
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v2/admin/team-members/{tenantId}` | List members |
| POST | `/api/v2/admin/team-members/{tenantId}` | Add member |
| POST | `/api/v2/admin/team-members/{tenantId}/{userId}` | Update role |
| DELETE | `/api/v2/admin/team-members/{tenantId}/{userId}` | Remove member |

### Products
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v2/admin/products` | List products |
| POST | `/api/v2/admin/products` | Create product |
| GET | `/api/v2/admin/products/{id}` | Get product |
| POST | `/api/v2/admin/products/{id}` | Update product |
| DELETE | `/api/v2/admin/products/{id}` | Delete product |

### Categories
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v2/admin/category` | List categories |
| POST | `/api/v2/admin/category/create` | Create category |
| PATCH | `/api/v2/admin/category/{id}` | Toggle publish |
| DELETE | `/api/v2/admin/category/{id}` | Delete category |

### Orders
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v2/admin/orders` | List orders |
| POST | `/api/v2/admin/orders` | Create order |
| GET | `/api/v2/admin/orders/{id}` | Get order |
| GET | `/api/v2/admin/orders/my-orders` | Get my orders |

### Webhooks
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v2/admin/webhooks/{tenantId}` | List webhooks |
| POST | `/api/v2/admin/webhooks/{tenantId}` | Create webhook |
| DELETE | `/api/v2/admin/webhooks/{tenantId}` | Delete webhook |

## 🔑 Headers

### Required for All Authenticated Requests
```bash
Authorization: Bearer YOUR_TOKEN
```

### Required for Tenant-Scoped Requests
```bash
Authorization: Bearer YOUR_TOKEN
X-Tenant-ID: 123
```

## 📦 Common Request Bodies

### Add Team Member
```json
{
  "email": "user@example.com",
  "role": "MEMBER"  // or "ADMIN"
}
```

### Create Product
```json
{
  "name": "Product Name",
  "description": "Product description",
  "price": 29.99,
  "stock": 100,
  "category_id": 5
}
```

### Create Category
```json
{
  "name": "Category Name",
  "description": "Category description"
}
```

### Create Order
```json
{
  "address_id": 2,
  "payment_type": 1,
  "total_amount": 99.99,
  "order_items": [
    {
      "product_id": 10,
      "qty": 2,
      "amount": 49.99,
      "size": "Large",
      "notes": "Extra packaging"
    }
  ]
}
```

### Create Webhook
```json
{
  "url": "https://your-app.com/webhooks",
  "events": ["user.registered", "order.created"]
}
```

## 🎯 Response Formats

### Success
```json
{
  "data": [...],
  "message": "Success"
}
```

### Error
```json
{
  "error": "Error message"
}
```

### Validation Error
```json
{
  "message": "Validation failed",
  "errors": {
    "field": ["Error message"]
  }
}
```

### Paginated
```json
{
  "data": [...],
  "current_page": 1,
  "per_page": 15,
  "total": 100
}
```

## 🔐 Roles

### ADMIN
- ✅ Add/remove team members
- ✅ Update member roles
- ✅ Manage products & categories
- ✅ Configure webhooks
- ✅ Update tenant settings

### MEMBER
- ✅ View team members
- ✅ Manage products & categories
- ✅ Process orders
- ❌ Cannot modify team
- ❌ Cannot change settings

## 🌐 Base URLs

- **Production**: `https://faisalshop.mvp-apps.ae`
- **Development**: `http://localhost:3000`

## 📊 HTTP Status Codes

| Code | Meaning |
|------|---------|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 422 | Validation Error |
| 429 | Too Many Requests |
| 500 | Server Error |

## 🔔 Webhook Events

- `user.registered` - User added to tenant
- `user.updated` - User info changed
- `user.removed` - User removed from tenant
- `order.created` - New order placed
- `order.updated` - Order status changed
- `product.created` - New product added
- `product.updated` - Product changed
- `category.created` - New category added

## 💡 Quick Tips

### Multi-Tenancy
- Always include `X-Tenant-ID` header for tenant-scoped requests
- Users can belong to multiple tenants
- Data is automatically filtered by tenant

### Authentication
- Token expires after inactivity
- Use `/who_am_i` to verify token validity
- Logout invalidates current token only

### Performance
- Use pagination for large datasets
- Products endpoint supports `page` and `per_page` params
- Default: 15 items per page

### Security
- Use HTTPS in production
- Verify webhook signatures
- Implement rate limiting
- Validate all inputs

## 📚 Documentation Links

- **Full Docs**: See `mint-docs/` folder
- **Getting Started**: `essentials/getting-started.mdx`
- **Architecture**: `essentials/architecture.mdx`
- **Multi-Tenancy**: `essentials/multi-tenancy.mdx`
- **Webhooks**: `essentials/webhooks-events.mdx`
- **OpenAPI Spec**: `api-reference/openapi-extended.json`

## 🛠️ Tools

### Test with cURL
```bash
curl -X GET https://faisalshop.mvp-apps.ae/api/v2/admin/products \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "X-Tenant-ID: 123"
```

### Test with JavaScript
```javascript
const response = await axios.get('/api/v2/admin/products', {
  headers: {
    'Authorization': `Bearer ${token}`,
    'X-Tenant-ID': tenantId
  }
});
```

### Generate Client
```bash
openapi-generator-cli generate \
  -i api-reference/openapi-extended.json \
  -g javascript \
  -o ./api-client
```

## 📞 Support

- **Email**: info@faisalkc.com
- **Docs**: `mint-docs/` folder
- **Laravel Docs**: `laravel-api/docs/` folder

---

**Version**: 2.1.0 | **Updated**: November 16, 2025

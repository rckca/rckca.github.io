<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>余江基地物资点单系统</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: "Microsoft YaHei", "PingFang SC", sans-serif;
        }
        
        body {
            background-color: #f5f5f5;
            color: #333;
            font-size: 14px;
            max-width: 100%;
            overflow-x: hidden;
        }
        
        /* 顶部导航栏 */
        .header {
            background-color: #1d6fa3;
            color: white;
            padding: 15px 10px;
            text-align: center;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            position: relative;
            z-index: 10;
        }
        
        .header h1 {
            font-size: 20px;
            font-weight: normal;
        }
        
        /* 主容器 */
        .container {
            display: flex;
            width: 100%;
            max-width: 100%;
            padding-top: 5px;
        }
        
        /* 左侧分类导航 */
        .category-nav {
            width: 25%;
            background: white;
            height: calc(100vh - 60px);
            position: sticky;
            top: 0;
            overflow-y: auto;
            box-shadow: 2px 0 5px rgba(0, 0, 0, 0.05);
            z-index: 5;
        }
        
        .category-nav ul {
            list-style: none;
            padding: 10px 0;
        }
        
        .category-nav li {
            padding: 12px 15px;
            font-size: 14px;
            border-left: 4px solid transparent;
            transition: all 0.3s;
            cursor: pointer;
        }
        
        .category-nav li.active,
        .category-nav li:hover {
            background-color: #eef7fd;
            color: #1d6fa3;
            border-left: 4px solid #1d6fa3;
        }
        
        /* 主商品区 */
        .main-content {
            flex: 1;
            padding: 10px 10px 80px;
            background-color: #f8f8f8;
            height: calc(100vh - 60px);
            overflow-y: auto;
        }
        
        .section-title {
            padding: 8px 10px;
            font-size: 16px;
            font-weight: bold;
            color: #1d6fa3;
            background-color: #eef7fd;
            margin: 10px 0;
            border-left: 3px solid #1d6fa3;
        }
        
        .items-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
        }
        
        .item-card {
            background: white;
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
            transition: transform 0.2s;
            display: flex;
            flex-direction: column;
            height: 100%;
        }
        
        .item-card:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }
        
        .item-image {
            height: 130px;
            background-color: #f0f9ff;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #1d6fa3;
            font-size: 36px;
        }
        
        .item-info {
            padding: 10px;
            flex-grow: 1;
        }
        
        .item-title {
            font-size: 14px;
            height: 40px;
            overflow: hidden;
            margin-bottom: 3px;
            line-height: 1.4;
        }
        
        .item-price {
            color: #e74c3c;
            font-weight: bold;
            font-size: 15px;
            margin-top: 5px;
        }
        
        .add-to-cart {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 8px;
        }
        
        .quantity-control {
            display: flex;
            align-items: center;
            background: #f0f9ff;
            border-radius: 20px;
            padding: 3px 8px;
        }
        
        .quantity-control button {
            width: 24px;
            height: 24px;
            border-radius: 50%;
            border: none;
            background: #1d6fa3;
            color: white;
            font-weight: bold;
            cursor: pointer;
            line-height: 1;
        }
        
        .quantity-control span {
            min-width: 26px;
            text-align: center;
        }
        
        /* 购物车 */
        .cart-container {
            position: fixed;
            bottom: 20px;
            right: 15px;
            z-index: 1000;
        }
        
        .cart-icon {
            width: 60px;
            height: 60px;
            background-color: #1d6fa3;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
        }
        
        .cart-icon i {
            font-size: 24px;
            color: white;
        }
        
        .cart-badge {
            position: absolute;
            top: -5px;
            right: -5px;
            background-color: #e74c3c;
            color: white;
            font-size: 12px;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .cart-panel {
            position: fixed;
            bottom: 85px;
            right: 15px;
            width: 90%;
            max-width: 400px;
            height: 70vh;
            background: white;
            border-radius: 10px;
            box-shadow: 0 -5px 15px rgba(0, 0, 0, 0.15);
            overflow: hidden;
            display: flex;
            flex-direction: column;
            transform: translateX(150%);
            transition: transform 0.3s ease-out;
        }
        
        .cart-panel.active {
            transform: translateX(0);
        }
        
        .cart-header {
            padding: 15px;
            background-color: #1d6fa3;
            color: white;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .cart-items {
            flex-grow: 1;
            overflow-y: auto;
            padding: 10px;
        }
        
        .cart-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 0;
            border-bottom: 1px solid #eee;
        }
        
        .cart-item-name {
            flex-grow: 1;
        }
        
        .cart-item-price {
            font-weight: bold;
            color: #e74c3c;
            margin-left: 10px;
        }
        
        .cart-footer {
            padding: 15px;
            background: #f8f8f8;
            border-top: 1px solid #eee;
        }
        
        .total-price {
            display: flex;
            justify-content: space-between;
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 15px;
        }
        
        .checkout-btn {
            width: 100%;
            padding: 12px;
            background-color: #1d6fa3;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
        }
        
        .checkout-btn:disabled {
            background-color: #ccc;
        }
        
        /* 订单结果页 */
        .order-result {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: white;
            z-index: 10000;
            padding: 20px;
            display: none;
            flex-direction: column;
        }
        
        .result-content {
            flex-grow: 1;
            overflow-y: auto;
            border: 1px solid #eee;
            padding: 20px;
            font-family: monospace;
            font-size: 16px;
            line-height: 1.8;
            white-space: pre;
        }
        
        .result-controls {
            padding: 15px;
            display: flex;
            gap: 15px;
        }
        
        .result-btn {
            flex: 1;
            padding: 12px;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
        }
        
        .back-btn {
            background-color: #e0e0e0;
            color: #333;
        }
        
        .copy-btn {
            background-color: #1d6fa3;
            color: white;
        }
        
        /* 响应式调整 */
        @media (max-width: 768px) {
            .items-grid {
                grid-template-columns: 1fr 1fr;
            }
            
            .item-title {
                font-size: 13px;
                height: 36px;
            }
            
            .item-price {
                font-size: 14px;
            }
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>余江基地物资点单系统</h1>
    </div>
    
    <div class="container">
        <!-- 左侧分类导航 -->
        <div class="category-nav">
            <ul>
                <li class="active" data-category="beverages">水及饮料</li>
                <li data-category="dairy">乳制品</li>
                <li data-category="noodles">方便面</li>
                <li data-category="bakery">面包饼干</li>
                <li data-category="beverage">冲泡类</li>
                <li data-category="snacks">散装食品</li>
            </ul>
        </div>
        
        <!-- 主商品区 -->
        <div class="main-content">
            <!-- 水及饮料 -->
            <div class="section-title" id="beverages">水及饮料</div>
            <div class="items-grid" id="beverages-grid">
                <!-- 饮料商品将通过JS动态生成 -->
            </div>
            
            <!-- 乳制品 -->
            <div class="section-title" id="dairy">乳制品</div>
            <div class="items-grid" id="dairy-grid">
                <!-- 乳制品商品将通过JS动态生成 -->
            </div>
            
            <!-- 方便面 -->
            <div class="section-title" id="noodles">方便面</div>
            <div class="items-grid" id="noodles-grid">
                <!-- 方便面商品将通过JS动态生成 -->
            </div>
            
            <!-- 面包饼干 -->
            <div class="section-title" id="bakery">面包饼干</div>
            <div class="items-grid" id="bakery-grid">
                <!-- 面包饼干商品将通过JS动态生成 -->
            </div>
            
            <!-- 冲泡类 -->
            <div class="section-title" id="beverage">冲泡类</div>
            <div class="items-grid" id="beverage-grid">
                <!-- 冲泡类商品将通过JS动态生成 -->
            </div>
            
            <!-- 散装食品 -->
            <div class="section-title" id="snacks">散装食品</div>
            <div class="items-grid" id="snacks-grid">
                <!-- 散装食品商品将通过JS动态生成 -->
            </div>
        </div>
    </div>
    
    <!-- 右下角购物车 -->
    <div class="cart-container">
        <div class="cart-icon" id="cartToggle">
            <i class="fas fa-shopping-cart"></i>
            <div class="cart-badge" id="cartCount">0</div>
        </div>
        
        <div class="cart-panel" id="cartPanel">
            <div class="cart-header">
                <h3>购物车</h3>
                <span>点击可修改数量</span>
            </div>
            <div class="cart-items" id="cartItems">
                <!-- 购物车项目将在此动态生成 -->
            </div>
            <div class="cart-footer">
                <div class="total-price">
                    <span>总计:</span>
                    <span id="cartTotal">￥0</span>
                </div>
                <button class="checkout-btn" id="checkoutBtn" disabled>提交订单</button>
            </div>
        </div>
    </div>
    
    <!-- 订单结果页面 -->
    <div class="order-result" id="orderResult">
        <h2 style="text-align:center; margin:15px 0; color:#1d6fa3;">订单详情</h2>
        <div class="result-content" id="orderContent">
            <!-- 订单详情将在此动态生成 -->
        </div>
        <div class="result-controls">
            <button class="result-btn back-btn" id="backBtn">返回修改</button>
            <button class="result-btn copy-btn" id="copyBtn">复制结果</button>
        </div>
    </div>

    <script>
        // 商品数据 - 包含您提供的所有商品
        const products = {
            beverages: [
                {id: 1, name: "佳德乐 (600ml*15)", price: 60, icon: "fas fa-wine-bottle"},
                {id: 2, name: "红牛 (250ml*24)", price: 125, icon: "fas fa-bolt"},
                {id: 3, name: "加多宝 (310ml*24)", price: 62, icon: "fas fa-cannabis"},
                {id: 4, name: "怡宝水 (555ml*24)", price: 28, icon: "fas fa-tint"},
                {id: 5, name: "中百事可乐 (500ml*24)", price: 60, icon: "fas fa-glass-whiskey"},
                {id: 6, name: "中美年达 (500ml*24)", price: 60, icon: "fas fa-glass-whiskey"},
                {id: 7, name: "听七喜 (330ml*24)", price: 46, icon: "fas fa-glass-whiskey"},
                {id: 8, name: "听百事可乐 (330ml*24)", price: 46, icon: "fas fa-glass-whiskey"},
                {id: 9, name: "听美年达 (330ml*24)", price: 46, icon: "fas fa-glass-whiskey"},
                {id: 10, name: "雀巢咖啡268ML (268ml*15)", price: 72, icon: "fas fa-coffee"},
                {id: 11, name: "东方树叶 (500ml*15)", price: 62, icon: "fas fa-leaf"},
                {id: 12, name: "中果粒 (450ml*24)", price: 76, icon: "fas fa-apple-alt"},
                {id: 13, name: "尖叫 (550ml*15)", price: 60, icon: "fas fa-bolt"},
                {id: 14, name: "东鹏特饮 (250ml*24)", price: 67, icon: "fas fa-bolt"},
                {id: 15, name: "脉动 (600ml*15)", price: 60, icon: "fas fa-bolt"},
                {id: 16, name: "元气森林外星人电解液 (500ml*15)", price: 72, icon: "fas fa-bolt"},
                {id: 17, name: "元气森林 (480ml*15)", price: 56, icon: "fas fa-bolt"},
                {id: 18, name: "旺仔牛奶 (245ml*12)", price: 58, icon: "fas fa-cow"},
                {id: 19, name: "喜多多 (200g*12)", price: 37, icon: "fas fa-cannabis"},
                {id: 20, name: "统一阿萨姆奶茶 (500ml*15)", price: 56, icon: "fas fa-mug-hot"},
                {id: 21, name: "统一红茶 (500ml*15)", price: 35, icon: "fas fa-mug-hot"},
                {id: 22, name: "怡宝菊花茶 (450ml*15)", price: 48, icon: "fas fa-mug-hot"},
                {id: 23, name: "银鹭八宝粥 (360g*12)", price: 39, icon: "fas fa-utensil-spoon"},
                {id: 24, name: "银鹭好粥道 (280g*12)", price: 45, icon: "fas fa-utensil-spoon"},
                {id: 25, name: "银鹭花生奶 (450ml*15)", price: 52, icon: "fas fa-glass-whiskey"},
                {id: 26, name: "银鹭银耳汤 (270g*12)", price: 43, icon: "fas fa-utensil-spoon"},
                {id: 27, name: "银鹭绿豆汤 (370g*12)", price: 43, icon: "fas fa-utensil-spoon"},
                {id: 28, name: "银鹭花生牛奶 (360g*12)", price: 43, icon: "fas fa-glass-whiskey"},
                {id: 29, name: "娃哈哈八宝粥 (360g*12)", price: 45, icon: "fas fa-utensil-spoon"},
                {id: 30, name: "娃哈哈营养快线 (350g*15)", price: 46, icon: "fas fa-glass-whiskey"},
                {id: 31, name: "娃哈哈AD钙奶 (450ml*15)", price: 65, icon: "fas fa-glass-whiskey"},
                {id: 32, name: "娃哈哈苏打水 (500ml*15)", price: 65, icon: "fas fa-tint"},
                {id: 33, name: "娃哈哈矿泉水 (596ml*24)", price: 28, icon: "fas fa-tint"},
                {id: 34, name: "娃哈哈矿泉水 (1.5L*12)", price: 29, icon: "fas fa-tint"},
                {id: 35, name: "苏萨椰子水 (333ml*15)", price: 85, icon: "fas fa-glass-whiskey"},
                {id: 36, name: "农夫山泉茶π (500ml*15)", price: 62, icon: "fas fa-mug-hot"},
                {id: 37, name: "润田翠 (500ml*24)", price: 28, icon: "fas fa-tint"},
                {id: 38, name: "1.5L怡宝水 (1.5L*12)", price: 29, icon: "fas fa-tint"},
                {id: 39, name: "4.5L怡宝水 (4.5L*4)", price: 29, icon: "fas fa-tint"}
            ],
            dairy: [
                {id: 40, name: "光明椰汁牛乳 (330ml*20)", price: 135, icon: "fas fa-glass-whiskey"},
                {id: 41, name: "光明噜渴奶 (458ml*20)", price: 155, icon: "fas fa-glass-whiskey"},
                {id: 42, name: "光明噜渴奶 (200ml*12)", price: 58, icon: "fas fa-glass-whiskey"},
                {id: 43, name: "蒙牛核桃奶 (250ml*24)", price: 70, icon: "fas fa-glass-whiskey"},
                {id: 44, name: "蒙牛纯牛奶 (250ml*24)", price: 65, icon: "fas fa-glass-whiskey"},
                {id: 45, name: "蒙牛特仑苏牛奶 (250ml*24)", price: 53, icon: "fas fa-glass-whiskey"},
                {id: 46, name: "伊利金典有机奶 (250ml*10)", price: 68, icon: "fas fa-glass-whiskey"},
                {id: 47, name: "味动力 (330ml*12)", price: 56, icon: "fas fa-glass-whiskey"},
                {id: 48, name: "伊利安慕希 (200g*10)", price: 60, icon: "fas fa-glass-whiskey"},
                {id: 49, name: "蒙牛纯甄 (200g*10)", price: 53, icon: "fas fa-glass-whiskey"}
            ],
            noodles: [
                {id: 50, name: "康师傅干拌面 (127g*12)", price: 65, icon: "fas fa-utensils"},
                {id: 51, name: "康师傅方便面 (108g*12)", price: 53, icon: "fas fa-utensils"},
                {id: 52, name: "统一干拌面 (120g*12)", price: 65, icon: "fas fa-utensils"},
                {id: 53, name: "统一方便面 (105g*12)", price: 53, icon: "fas fa-utensils"},
                {id: 54, name: "陈村火鸡面 (100g*12)", price: 65, icon: "fas fa-utensils"},
                {id: 55, name: "陈村螺丝粉 (120g*12)", price: 58, icon: "fas fa-utensils"},
                {id: 56, name: "食族人酸辣粉 (130g*12)", price: 156, icon: "fas fa-utensils"},
                {id: 57, name: "康师傅金汤肥牛 (113g*12)", price: 60, icon: "fas fa-utensils"},
                {id: 58, name: "陈村酸辣粉 (100g*12)", price: 60, icon: "fas fa-utensils"},
                {id: 59, name: "汤达人 (82g*12)", price: 60, icon: "fas fa-utensils"}
            ],
            bakery: [
                {id: 60, name: "奥利奥饼干 (97g*24)", price: 156, icon: "fas fa-cookie"},
                {id: 61, name: "旺旺雪饼 (84g*20)", price: 138, icon: "fas fa-cookie"},
                {id: 62, name: "小白心里软 (1 * 4斤)", price: 90, icon: "fas fa-bread-slice"},
                {id: 63, name: "沃隆坚果 (1 * 750克)", price: 100, icon: "fas fa-seedling"},
                {id: 64, name: "每日坚果 (25g*30)", price: 100, icon: "fas fa-seedling"},
                {id: 65, name: "恰恰坚果 (20g*14)", price: 95, icon: "fas fa-seedling"},
                {id: 66, name: "好想你红枣礼盒 (特级)(30g*30)", price: 88, icon: "fas fa-gift"},
                {id: 67, name: "双汇香辣肠 (1.92kg)", price: 60, icon: "fas fa-drumstick-bite"},
                {id: 68, name: "双汇玉米肠 (1.92kg)", price: 60, icon: "fas fa-drumstick-bite"},
                {id: 69, name: "双汇王中王火腿肠 (3.6kg)", price: 110, icon: "fas fa-drumstick-bite"},
                {id: 70, name: "德芙 (43g*12)", price: 105, icon: "fas fa-candy-cane"},
                {id: 71, name: "鸽鸽豆角干 (5kg)", price: 175, icon: "fas fa-seedling"},
                {id: 72, name: "绿箭 (1 * 20条)", price: 38, icon: "fas fa-candy-cane"},
                {id: 73, name: "达利园瑞士卷 (1 * 2.5kg)", price: 95, icon: "fas fa-cookie"},
                {id: 74, name: "达利园蛋黄派 (1 * 2.5kg)", price: 82, icon: "fas fa-cookie"},
                {id: 75, name: "达利园小面包 (1 * 2.5kg)", price: 92, icon: "fas fa-bread-slice"},
                {id: 76, name: "达利园软面包 (1 * 2.5kg)", price: 92, icon: "fas fa-bread-slice"},
                {id: 77, name: "谷友佳手撕面包 (1 * 2.5kg)", price: 88, icon: "fas fa-bread-slice"},
                {id: 78, name: "吐司面包 (1 * 2.5kg)", price: 88, icon: "fas fa-bread-slice"}
            ],
            beverage: [
                {id: 79, name: "正山小种 (1 * 100g)", price: 78, icon: "fas fa-mug-hot"},
                {id: 80, name: "晓起黄菊 (8朵)", price: 80, icon: "fas fa-mug-hot"},
                {id: 81, name: "茗茶 (1 * 1)", price: 150, icon: "fas fa-mug-hot"},
                {id: 82, name: "婺源高山绿茶 (1 * 250g)", price: 50, icon: "fas fa-mug-hot"},
                {id: 83, name: "金骏眉 (1 * 150g)", price: 70, icon: "fas fa-mug-hot"},
                {id: 84, name: "雀巢咖啡 (7 * 13g)", price: 12, icon: "fas fa-coffee"},
                {id: 85, name: "香飘飘奶茶 (1 * 30)", price: 93, icon: "fas fa-mug-hot"}
            ],
            snacks: [
                {id: 86, name: "好丽友蛋黄派 (138g/盒)", price: 11.5, icon: "fas fa-cookie"},
                {id: 87, name: "好丽友巧克力 (222g/盒)", price: 11.5, icon: "fas fa-candy-cane"},
                {id: 88, name: "好丽友薯愿蜂蜜牛奶 (104g/盒)", price: 8, icon: "fas fa-cookie"},
                {id: 89, name: "士力架 (1 * 400g)", price: 45, icon: "fas fa-candy-cane"},
                {id: 90, name: "可比克 (1 * 105g)", price: 8, icon: "fas fa-cookie"},
                {id: 91, name: "锐源阿姣枣 (1 * 200g)", price: 3.5, icon: "fas fa-apple-alt"},
                {id: 92, name: "牡丹亭多味花生 (1 * 60g)", price: 2.5, icon: "fas fa-seedling"},
                {id: 93, name: "曹师傅麻辣腿 (1 * 100g)", price: 6.5, icon: "fas fa-drumstick-bite"},
                {id: 94, name: "香之派香辣鸭掌 (1 * 30g)", price: 2, icon: "fas fa-drumstick-bite"},
                {id: 95, name: "美好时光海苔 (1 * 4.5g)", price: 4.5, icon: "fas fa-leaf"},
                {id: 96, name: "溜溜梅 (1 * 60g)", price: 5, icon: "fas fa-apple-alt"},
                {id: 97, name: "郑新初牛肉干 (1 * 40g)", price: 7.5, icon: "fas fa-drumstick-bite"},
                {id: 98, name: "甄好曲奇 (1 * 93g)", price: 4.5, icon: "fas fa-cookie"},
                {id: 99, name: "达利园香葱饼 (1 * 130g)", price: 4, icon: "fas fa-cookie"},
                {id: 100, name: "好吃点 (1 * 108g)", price: 4.5, icon: "fas fa-cookie"},
                {id: 101, name: "甘源每日豆果 (1 * 168g)", price: 15, icon: "fas fa-seedling"},
                {id: 102, name: "甘源蟹黄蚕豆 (1 * 75g)", price: 4.5, icon: "fas fa-seedling"},
                {id: 103, name: "甘源蟹黄瓜子 (1 * 75g)", price: 4.5, icon: "fas fa-seedling"},
                {id: 104, name: "天之湘鸭掌 (1 * 90g)", price: 10, icon: "fas fa-drumstick-bite"},
                {id: 105, name: "天之湘鸭翅 (1 * 60g)", price: 8, icon: "fas fa-drumstick-bite"},
                {id: 106, name: "无穷酱卤鸡翅根 (20g*20袋)", price: 80, icon: "fas fa-drumstick-bite"},
                {id: 107, name: "无穷酱卤鸭翅根 (30g*16袋)", price: 72, icon: "fas fa-drumstick-bite"},
                {id: 108, name: "无穷卤鸡爪 (1 * 75g)", price: 14, icon: "fas fa-drumstick-bite"},
                {id: 109, name: "无穷爱辣鸡腿 (1 * 70g)", price: 14, icon: "fas fa-drumstick-bite"},
                {id: 110, name: "卫龙大辣棒 (*10)", price: 45, icon: "fas fa-carrot"},
                {id: 111, name: "卫龙魔芋爽 (50g*10)", price: 45, icon: "fas fa-carrot"},
                {id: 112, name: "好丽友呀土豆牛排味 (1 * 40g)", price: 4, icon: "fas fa-cookie"},
                {id: 113, name: "好丽友呀土豆蕃茄味 (1 * 40g)", price: 4, icon: "fas fa-cookie"},
                {id: 114, name: "寻香记糯米锅巴 (1 * 105g)", price: 2.5, icon: "fas fa-cookie"},
                {id: 115, name: "香舟金针菇 (26g*20)", price: 24, icon: "fas fa-seedling"},
                {id: 116, name: "恒康碧根果仁 (1 * 108g)", price: 33, icon: "fas fa-seedling"},
                {id: 117, name: "恒康琥珀桃仁 (1 * 125g)", price: 19, icon: "fas fa-seedling"},
                {id: 118, name: "恒康蜂蜜桃仁 (1 * 125g)", price: 19, icon: "fas fa-seedling"},
                {id: 119, name: "恒康香酥腰果 (1 * 120g)", price: 22, icon: "fas fa-seedling"},
                {id: 120, name: "青豪园西梅 (1 * 180g)", price: 10, icon: "fas fa-apple-alt"},
                {id: 121, name: "青豪园老婆梅 (1 * 210g)", price: 10, icon: "fas fa-apple-alt"},
                {id: 122, name: "青豪园橄榄 (1 * 235g)", price: 10, icon: "fas fa-apple-alt"},
                {id: 123, name: "青豪园杨梅 (1 * 190g)", price: 10, icon: "fas fa-apple-alt"},
                {id: 124, name: "爱尚咪咪虾条 (1 * 180g)", price: 6, icon: "fas fa-cookie"},
                {id: 125, name: "零零亲炭秘卤锁骨 (1 * 65g)", price: 8, icon: "fas fa-drumstick-bite"},
                {id: 126, name: "零零亲炭秘卤鸭翅根 (1 * 70g)", price: 8, icon: "fas fa-drumstick-bite"},
                {id: 127, name: "零零亲炭秘卤鸭脖 (1 * 70g)", price: 8, icon: "fas fa-drumstick-bite"},
                {id: 128, name: "吴哥酒意酒鬼花生 (1 * 155g)", price: 5, icon: "fas fa-seedling"},
                {id: 129, name: "伊道香泡椒凤爪 (1 * 100g)", price: 5, icon: "fas fa-drumstick-bite"},
                {id: 130, name: "瑶红麻辣猪蹄 (1 * 150g)", price: 14, icon: "fas fa-drumstick-bite"},
                {id: 131, name: "王老大麻辣猪蹄 (1 * 135g)", price: 14, icon: "fas fa-drumstick-bite"},
                {id: 132, name: "啃仔鸭爪 (105g)(1 * 105g)", price: 8, icon: "fas fa-drumstick-bite"},
                {id: 133, name: "王子辣条 (110g)(1 * 110g)", price: 6, icon: "fas fa-carrot"},
                {id: 134, name: "港州芒果干 (1 * 90g)", price: 11, icon: "fas fa-apple-alt"},
                {id: 135, name: "青豪园草莓干 (1 * 70g)", price: 11, icon: "fas fa-apple-alt"},
                {id: 136, name: "无穷盐焗蛋 (个)", price: 2, icon: "fas fa-egg"},
                {id: 137, name: "奶酪棒妙可蓝多 (1 * 90g)", price: 18, icon: "fas fa-cheese"},
                {id: 138, name: "义生隆面包 (1 * 5个)", price: 12.5, icon: "fas fa-bread-slice"},
                {id: 139, name: "益达 (1 * 56g)", price: 11, icon: "fas fa-candy-cane"},
                {id: 140, name: "零食街香蕉片 (1 * 80g)", price: 6, icon: "fas fa-apple-alt"},
                {id: 141, name: "礼良早餐包 (1 * 80g)", price: 4, icon: "fas fa-bread-slice"},
                {id: 142, name: "老北京软麻花 (1 * 180g)", price: 6, icon: "fas fa-cookie"},
                {id: 143, name: "榕盛一帆绿豆饼 (1 * 90g)", price: 3, icon: "fas fa-cookie"},
                {id: 144, name: "力诚炭烤小香肠 (1 * 72g)", price: 10, icon: "fas fa-drumstick-bite"},
                {id: 145, name: "零零亲猪肉脯 (1 * 65g)", price: 13, icon: "fas fa-drumstick-bite"},
                {id: 146, name: "零零亲鲜香牛肉 (1 * 72g)", price: 13, icon: "fas fa-drumstick-bite"},
                {id: 147, name: "零零亲手撕肉条 (1 * 60g)", price: 13, icon: "fas fa-drumstick-bite"},
                {id: 148, name: "泰国脆脆条 (1 * 75g)", price: 7, icon: "fas fa-cookie"},
                {id: 149, name: "天之湘素牛排 (1 * 80g)", price: 8, icon: "fas fa-carrot"},
                {id: 150, name: "天之湘辣子鸡丁 (1 * 80g)", price: 8, icon: "fas fa-drumstick-bite"},
                {id: 151, name: "天之湘虎皮凤爪 (1 * 90g)", price: 10, icon: "fas fa-drumstick-bite"},
                {id: 152, name: "天之湘手撕肉干 (1 * 45g)", price: 8, icon: "fas fa-drumstick-bite"},
                {id: 153, name: "天之湘风干烤脖 (1 * 70g)", price: 8, icon: "fas fa-drumstick-bite"},
                {id: 154, name: "天之湘酒鬼鱼仔 (1 * 50g)", price: 8, icon: "fas fa-fish"},
                {id: 155, name: "富食村去骨凤爪 (1 * 80g)", price: 10, icon: "fas fa-drumstick-bite"},
                {id: 156, name: "小飞燕汤泡臭豆腐 (1 * 108g)", price: 5, icon: "fas fa-drumstick-bite"},
                {id: 157, name: "与美手剥笋 (1 * 200g)", price: 6, icon: "fas fa-carrot"},
                {id: 158, name: "竹香园泡椒笋尖 (1 * 100g)", price: 4, icon: "fas fa-carrot"},
                {id: 159, name: "依夫南瓜干 (1 * 100g)", price: 4, icon: "fas fa-carrot"},
                {id: 160, name: "依夫茄子干 (1 * 100g)", price: 4, icon: "fas fa-carrot"},
                {id: 161, name: "齐云山枣糕 (1 * 300g)", price: 25, icon: "fas fa-cookie"},
                {id: 162, name: "齐云山枣糕 (1 * 150g)", price: 12.5, icon: "fas fa-cookie"},
                {id: 163, name: "乐事 (1 * 70g)", price: 7, icon: "fas fa-cookie"},
                {id: 164, name: "纳宝帝威化饼 (1 * 145g)", price: 7, icon: "fas fa-cookie"},
                {id: 165, name: "维德麻花 (1 * 160g)", price: 4, icon: "fas fa-cookie"},
                {id: 166, name: "好吃点海苔饼 (1 * 130g)", price: 4.5, icon: "fas fa-cookie"},
                {id: 167, name: "星启成黑麦苏打饼 (1 * 108g)", price: 5, icon: "fas fa-cookie"},
                {id: 168, name: "香薯爷红薯干 (1 * 250g)", price: 5, icon: "fas fa-apple-alt"},
                {id: 169, name: "品优格唱片面包 (1 * 100g)", price: 4, icon: "fas fa-bread-slice"},
                {id: 170, name: "礼良肉松面包 (1 * 80g)", price: 4, icon: "fas fa-bread-slice"},
                {id: 171, name: "好印象中华杨梅 (1 * 55g)", price: 3, icon: "fas fa-apple-alt"},
                {id: 172, name: "鸽鸽牛肉干 (1 * 50g)", price: 10, icon: "fas fa-drumstick-bite"},
                {id: 173, name: "零食街紫薯仔 (1 * 130g)", price: 5, icon: "fas fa-apple-alt"},
                {id: 174, name: "星启成芝士小脆 (1 * 90g)", price: 5, icon: "fas fa-cookie"},
                {id: 175, name: "味中湘鸭肠 (1 * 60g)", price: 7, icon: "fas fa-drumstick-bite"},
                {id: 176, name: "味中湘鸭胗 (1 * 60g)", price: 7, icon: "fas fa-drumstick-bite"},
                {id: 177, name: "甘源青豌豆 (1 * 75g)", price: 5, icon: "fas fa-seedling"},
                {id: 178, name: "长龙湘脆藕片 (1 * 120g)", price: 8, icon: "fas fa-carrot"},
                {id: 179, name: "长龙杏鲍菇鱿鱼 (1 * 85g)", price: 8, icon: "fas fa-fish"},
                {id: 180, name: "长龙香脆海带丝 (1 * 120g)", price: 8, icon: "fas fa-leaf"},
                {id: 181, name: "好丽友木糖醇 (1 * 101g)", price: 16, icon: "fas fa-candy-cane"},
                {id: 182, name: "鸽鸽豆角干 (1 * 69g)", price: 2.5, icon: "fas fa-carrot"},
                {id: 183, name: "鸽鸽多味花生 (1 * 70g)", price: 5, icon: "fas fa-seedling"},
                {id: 184, name: "甜畅海螺酥 (1 * 90g)", price: 2.5, icon: "fas fa-cookie"},
                {id: 185, name: "十三妹素沙丁鱼 (1 * 55g)", price: 3, icon: "fas fa-fish"},
                {id: 186, name: "零食街跳跳奶棒糖 (1 * 46g)", price: 5, icon: "fas fa-candy-cane"},
                {id: 187, name: "绿活牛肉粒 (1 * 100g)", price: 10, icon: "fas fa-drumstick-bite"},
                {id: 188, name: "佰味益品卤鹌鹑蛋 (1 * 65g)", price: 4, icon: "fas fa-egg"},
                {id: 189, name: "乡思缘去皮鸡腿 (1 * 77g)", price: 6, icon: "fas fa-drumstick-bite"},
                {id: 190, name: "乡思缘鸡胸肉 (98.5g)", price: 6, icon: "fas fa-drumstick-bite"},
                {id: 191, name: "乡思缘风干大鸭腿 (1 * 100g)", price: 8, icon: "fas fa-drumstick-bite"},
                {id: 192, name: "伊拼拼泡椒凤爪 (1 * 60g)", price: 2.5, icon: "fas fa-drumstick-bite"},
                {id: 193, name: "艾娃板栗饼 (1 * 225g)", price: 6, icon: "fas fa-cookie"},
                {id: 194, name: "友利多椰蓉面包 (1 * 200g)", price: 6, icon: "fas fa-bread-slice"},
                {id: 195, name: "肖天奇腿型面包 (1 * 260g)", price: 7, icon: "fas fa-bread-slice"}
            ]
        };

        // 全局状态
        const cart = {};
        
        // DOM 元素
        const cartToggle = document.getElementById('cartToggle');
        const cartPanel = document.getElementById('cartPanel');
        const cartItems = document.getElementById('cartItems');
        const cartCount = document.getElementById('cartCount');
        const cartTotal = document.getElementById('cartTotal');
        const checkoutBtn = document.getElementById('checkoutBtn');
        const orderResult = document.getElementById('orderResult');
        const orderContent = document.getElementById('orderContent');
        const backBtn = document.getElementById('backBtn');
        const copyBtn = document.getElementById('copyBtn');
        
        // 初始化页面
        document.addEventListener('DOMContentLoaded', () => {
            // 生成所有商品
            generateProducts();
            
            // 导航点击事件
            document.querySelectorAll('.category-nav li').forEach(item => {
                item.addEventListener('click', function() {
                    const category = this.dataset.category;
                    
                    // 更新激活状态
                    document.querySelectorAll('.category-nav li').forEach(li => {
                        li.classList.remove('active');
                    });
                    this.classList.add('active');
                    
                    // 滚动到对应分类
                    const section = document.getElementById(category);
                    if (section) {
                        section.scrollIntoView({ behavior: 'smooth', block: 'start' });
                    }
                });
            });
            
            // 购物车开关
            cartToggle.addEventListener('click', () => {
                cartPanel.classList.toggle('active');
            });
            
            // 提交订单按钮
            checkoutBtn.addEventListener('click', generateOrder);
            
            // 返回和复制按钮
            backBtn.addEventListener('click', () => {
                orderResult.style.display = 'none';
            });
            
            copyBtn.addEventListener('click', () => {
                const text = orderContent.innerText;
                navigator.clipboard.writeText(text).then(() => {
                    alert('订单内容已复制到剪贴板！');
                });
            });
            
            // 初始化购物车为空提示
            cartItems.innerHTML = '<div style="text-align:center; padding:40px; color:#999;">购物车为空</div>';
        });
        
        // 生成所有商品
        function generateProducts() {
            for (const category in products) {
                const grid = document.getElementById(`${category}-grid`);
                if (!grid) continue;
                
                grid.innerHTML = '';
                
                products[category].forEach(product => {
                    const itemCard = document.createElement('div');
                    itemCard.className = 'item-card';
                    itemCard.dataset.id = product.id;
                    itemCard.dataset.name = product.name;
                    itemCard.dataset.price = product.price;
                    
                    itemCard.innerHTML = `
                        <div class="item-image">
                            <i class="${product.icon}"></i>
                        </div>
                        <div class="item-info">
                            <div class="item-title">${product.name}</div>
                            <div class="item-price">￥${product.price}</div>
                            <div class="add-to-cart">
                                <span class="quantity-control">
                                    <button class="decrease">-</button>
                                    <span class="quantity">0</span>
                                    <button class="increase">+</button>
                                </span>
                            </div>
                        </div>
                    `;
                    
                    grid.appendChild(itemCard);
                    
                    // 添加事件监听
                    const quantityControl = itemCard.querySelector('.quantity-control');
                    quantityControl.querySelector('.increase').addEventListener('click', (e) => {
                        e.stopPropagation();
                        addToCart(product.id);
                        updateCartUI();
                    });
                    
                    quantityControl.querySelector('.decrease').addEventListener('click', (e) => {
                        e.stopPropagation();
                        removeFromCart(product.id);
                        updateCartUI();
                    });
                });
            }
        }
        
        // 添加到购物车
        function addToCart(id) {
            // 在所有分类中查找商品
            let product = null;
            for (const category in products) {
                const found = products[category].find(p => p.id === id);
                if (found) {
                    product = found;
                    break;
                }
            }
            
            if (!product) return;
            
            if (cart[id]) {
                cart[id].quantity++;
            } else {
                cart[id] = {
                    id: product.id,
                    name: product.name,
                    price: product.price,
                    quantity: 1
                };
            }
            
            // 更新卡片数量显示
            const quantityEl = document.querySelector(`.item-card[data-id="${id}"] .quantity`);
            if (quantityEl) {
                quantityEl.textContent = cart[id].quantity;
            }
        }
        
        // 从购物车移除
        function removeFromCart(id) {
            if (cart[id] && cart[id].quantity > 1) {
                cart[id].quantity--;
                
                // 更新卡片数量显示
                const quantityEl = document.querySelector(`.item-card[data-id="${id}"] .quantity`);
                if (quantityEl) {
                    quantityEl.textContent = cart[id].quantity;
                }
            } else if (cart[id] && cart[id].quantity === 1) {
                delete cart[id];
                
                // 更新卡片数量显示
                const quantityEl = document.querySelector(`.item-card[data-id="${id}"] .quantity`);
                if (quantityEl) {
                    quantityEl.textContent = 0;
                }
            }
        }
        
        // 更新购物车UI
        function updateCartUI() {
            // 更新购物车数量徽章
            const totalItems = Object.values(cart).reduce((sum, item) => sum + item.quantity, 0);
            cartCount.textContent = totalItems;
            
            // 更新购物车列表
            cartItems.innerHTML = '';
            if (totalItems === 0) {
                cartItems.innerHTML = '<div style="text-align:center; padding:40px; color:#999;">购物车为空</div>';
                cartTotal.textContent = '￥0';
                checkoutBtn.disabled = true;
                return;
            }
            
            // 启用提交按钮
            checkoutBtn.disabled = false;
            
            // 计算总金额
            let total = 0;
            
            // 为每个商品创建购物车项目
            Object.values(cart).forEach(item => {
                const cartItemEl = document.createElement('div');
                cartItemEl.className = 'cart-item';
                
                const subtotal = item.price * item.quantity;
                total += subtotal;
                
                cartItemEl.innerHTML = `
                    <div class="cart-item-name">${item.name}</div>
                    <div class="cart-item-quantity">
                        <button class="decrease" data-id="${item.id}">-</button>
                        <span>${item.quantity}</span>
                        <button class="increase" data-id="${item.id}">+</button>
                    </div>
                    <div class="cart-item-price">￥${formatPrice(subtotal)}</div>
                `;
                
                cartItems.appendChild(cartItemEl);
                
                // 添加数量调整按钮事件
                cartItemEl.querySelector('.increase').addEventListener('click', () => {
                    addToCart(item.id);
                    updateCartUI();
                });
                
                cartItemEl.querySelector('.decrease').addEventListener('click', () => {
                    removeFromCart(item.id);
                    updateCartUI();
                });
            });
            
            // 更新总金额
            cartTotal.textContent = `￥${formatPrice(total)}`;
        }
        
        // 格式化价格（保留一位小数，如无小数位则不显示）
        function formatPrice(price) {
            const fixed = price.toFixed(1);
            return fixed.endsWith('.0') ? fixed.slice(0, -2) : fixed;
        }
        
        // 生成订单（输出结果）
        function generateOrder() {
            let resultText = '';
            let total = 0;
            
            Object.values(cart).forEach(item => {
                const itemTotal = item.price * item.quantity;
                total += itemTotal;
                
                resultText += `${item.name},${item.quantity},${formatPrice(item.price)},${formatPrice(itemTotal)}\n`;
            });
            
            resultText += `\n订单总计: ${formatPrice(total)}元`;
            
            // 更新订单详情显示
            orderContent.textContent = resultText;
            
            // 显示结果页面
            orderResult.style.display = 'flex';
            
            // 隐藏购物车面板
            cartPanel.classList.remove('active');
        }
    </script>
</body>
</html>

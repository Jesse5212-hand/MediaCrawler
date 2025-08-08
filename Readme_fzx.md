要在本地使用conda环境部署MediaCrawler项目，可按以下步骤操作，结合项目依赖需求与conda环境管理特点进行配置：


### 一、前期准备
1. **安装必要工具**  
   - 确保已安装 **conda**（建议Anaconda或Miniconda），用于创建和管理虚拟环境。  
   - 若需爬取抖音或知乎，需提前安装 **Node.js**（版本≥16），可从[Node.js官网](https://nodejs.org/)下载安装。  
   - 克隆或下载MediaCrawler项目到本地，进入项目根目录（后续操作均在此目录下进行）。


### 二、创建并配置conda虚拟环境
1. **创建conda环境**  
   项目依赖基于Python 3.9.6，因此指定Python版本创建环境：  
   ```bash
   conda create -n mediacrawler python=3.9.6 -y
   ```  
   （`mediacrawler`为环境名称，可自定义）

2. **激活conda环境**  
   ```bash
   # Windows
   conda activate mediacrawler

   # macOS & Linux
   source activate mediacrawler
   ```


### 三、安装项目依赖
1. **安装Python依赖库**  
   项目依赖记录在`requirements.txt`中，使用pip安装（conda环境中可直接使用pip）：  
   ```bash
   pip install -r requirements.txt
   ```  
   *若出现版本冲突，可尝试手动调整冲突库的版本（参考错误提示）。*

2. **安装playwright浏览器驱动**  
   执行以下命令安装playwright所需的浏览器驱动（如Chromium、Firefox等）：  
   ```bash
   playwright install
   ```


### 四、数据库配置（可选，根据存储需求）
若需使用MySQL或SQLite存储数据，需提前进行对应配置：  
1. **MySQL配置（适合多用户或大规模数据）**  
   - 提前在本地或服务器创建MySQL数据库（数据库名可自定义，如`mediacrawler`）。  
   - 首次使用时，执行以下命令初始化数据库表结构：  
     ```bash
     python db.py
     ```  
   - 运行爬虫时通过`--save_data_option db`指定MySQL存储（需确保`config`中数据库连接信息正确，如主机、用户名、密码等）。  

2. **SQLite配置（适合个人或小规模数据，推荐新手）**  
   - 无需额外安装数据库，直接在运行命令中添加`--save_data_option sqlite`即可启用，数据库文件会自动生成在项目目录的`schema/sqlite_tables.db`。  


### 五、运行爬虫程序
1. **查看支持的平台和参数**  
   执行以下命令查看所有可用的平台（如小红书、抖音等）和参数说明：  
   ```bash
   python main.py --help
   ```  

2. **示例：爬取小红书内容**  
   - 从配置文件读取关键词搜索帖子（需扫码登录）：  
     ```bash
     python main.py --platform xhs --lt qrcode --type search
     ```  
   - 保存到SQLite（推荐个人使用）：  
     ```bash
     python main.py --platform xhs --lt qrcode --type search --save_data_option sqlite
     ```  


### 六、注意事项
- **环境激活**：每次运行项目前，需确保已通过`conda activate mediacrawler`激活环境。  
- **配置修改**：如需开启评论爬取，需在`config/base_config.py`中修改`ENABLE_GET_COMMENTS`变量为`True`，其他功能可参考该文件的中文注释调整。  
- **数据存储**：CSV和JSON文件会自动保存到项目的`data/`目录下，无需额外配置。

通过以上步骤，即可使用conda环境在本地成功部署并运行MediaCrawler项目。

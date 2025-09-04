---
title: Obsidian插件发布Typecho文章
description: 制作Obsidian插件，适配PC/Andriod
published: true
date: 2025-09-04T09:07:18.463Z
tags: typecho, obsidian
editor: markdown
dateCreated: 2025-09-04T09:07:18.463Z
---


# 前言

很显然，这篇文章是使用我自己制作的obsidian插件发布的文章，既可以使用手机版本的obsidian也可以在windows上面使用obsidian进行发送文章，先设置obdsidian插件比如xmlrpc的链接，账号密码等。
![image.png](https://cdn.hansjack.com/2025/08/b0afbe610b9a592ca83f120b30b11f54.webp)


## 一、插件说明

为了防止文件上传出错，我并没有加文件上传的逻辑，不过可以使用piclist等插件继续上传文件。

### 1、功能要点

- 使用 **XML-RPC** 对接 Typecho，支持：
    
    - 标题 `title`
        
    - 自定义路径 `slug`
        
    - 标签 `tags`（数组或逗号分隔）
        
    - 分类 `categories`（可选）
        
    - 文章内容（当前笔记正文）
        
    - 是否草稿 `draft`（发布时 publish=false）
        
- 首次发布会把返回的 **cid** 写回到笔记 Frontmatter（默认键名 `cid`），二次发布自动改为 `editPost` 更新。
    
- 支持自行映射 Frontmatter 字段名（如把 `tags` 改成 `blogtags` 等）。
    

### 2、安装与配置

**插件文件夹名：typecho-xmlrpc-publisher**

1. 下载最新版本，解压 ZIP，把整个 `typecho-xmlrpc-publisher` 文件夹放到你的库目录：`<你的库>/.obsidian/plugins/`
    
2. 在 Obsidian 设置 → 第三方插件，启用 **Typecho XML-RPC Publisher**。

3. typecho需要的配置：
![image.png](https://cdn.hansjack.com/2025/08/b99c56bc2c332fd1062924c51b14a307.webp)
![image.png](https://cdn.hansjack.com/2025/08/c026135e1d5059dac1d744c693aec5f6.webp)

4. obsidian最新版本配置：
![image.png](https://cdn.hansjack.com/2025/08/da15980c3a874c6d56d105d906d12e0f.webp)

5. 版本下载最新1.3版本(1.0是打包文件，后续**只更新main.js文件**，只发布代码，**代码在更新内容后面**)，添加mainifest.json文件在插件里面：

```json
{
  "id": "typecho-xmlrpc-publisher",
  "name": "Typecho XML-RPC Publisher",
  "version": "1.3.0",
  "minAppVersion": "1.5.0",
  "description": "通过 XML-RPC （MetaWeblog） 将当前笔记发布到 Typecho 博客。支持标题、slug、标签、类别、草稿；可选是否更新发布时间；支持强制同步服务器发布时间。",
  "author": "Hans J. Han",
  "authorUrl": "https://www.hansjack.com",
  "isDesktopOnly": false,
  "js": "main.js"
}
```

6. 最终插件文件夹有：![image.png](https://cdn.hansjack.com/2025/08/a056983428374ed65e1d5a7239c99b99.webp)
7. 如果样式错乱，可以添加styles.css文件：
```css
.post-card, .comment-card {
    border: 1px solid var(--background-modifier-border);
    border-radius: 6px;
    padding: 10px;
    margin-bottom: 10px;
}

.post-header, .comment-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.post-title {
    font-weight: bold;
    color: var(--text-accent);
    text-decoration: none;
}

.post-meta {
    font-size: 12px;
    color: var(--text-muted);
    margin-top: 4px;
}

.post-actions, .comment-actions {
    display: flex;
    gap: 6px; /* 按钮间距 */
}

.action-btn {
    padding: 2px 8px;
    border-radius: 4px;
    border: 1px solid var(--background-modifier-border);
    background: var(--interactive-normal);
    cursor: pointer;
}

    .action-btn:hover {
        background: var(--interactive-hover);
    }
```

8. 其他配置默认即可：

**参考我的Typecho配置**：

![image.png](https://cdn.hansjack.com/2025/08/1b6a5e3ff6959e0303a960796d1b0d7d.webp)


- **Endpoint**：Typecho 的 XML-RPC 入口（常见为 `https://你的站点/action/xmlrpc`，需后台启用 MetaWeblog/XML-RPC 功能）
    
- **Username / Password**：Typecho 后台账号
    
- **Default Blog ID**：填 `0`
    
- **Frontmatter 映射**：可修改 `title/slug/tags/categories/draft/cid` 对应的键名
    

### 3、使用说明

参考文章：
![Snipaste_2025-08-21_13-16-34.webp](https://cdn.hansjack.com/2025/08/cfb1f8471d9893c220717d44548f5d3a.webp)


```txt

---
title:
slug:
tags:
categories:
draft: false
---

```

> 确保前面属性正确！



- 打开要发布的笔记，运行命令：**Publish current note to Typecho**
    
- 弹窗里确认或补齐：标题、slug、标签、分类、是否草稿
    
- 首发成功后，`cid` 会自动写入 Frontmatter；之后再发布会自动走更新流程


## 二、插件下载

### 1、版本1.0：存在发布时间错误
下面是1.0版本：存在更新文章时间跟随当前时间的错误逻辑
[typecho-xmlrpc-publisher.zip](https://www.hansjack.com/usr/uploads/2025/08/261350796.zip)

### 2、版本1.1：修复发布时间错误/添加时间偏移纠正
下面是1.1版本：添加是否当前时间作为发布时间的选择、添加时间偏移（同步时间、发布时间）
![image.png](https://cdn.hansjack.com/2025/08/f399cd21f5b176780bf03346199f4e25.webp)


插件文件夹名：typecho-xmlrpc-publisher

main.js文件代码如下：
```javascript
// Typecho XML-RPC 发布器 - main.js

const { Plugin, Modal, Setting, Notice, requestUrl } = require('obsidian');

  

class XmlRpc {

  static iso8601(date) {

    const pad = (n) => (n < 10 ? '0' + n : '' + n);

    return date.getFullYear().toString() +

      pad(date.getMonth() + 1) +

      pad(date.getDate()) + 'T' +

      pad(date.getHours()) + ':' +

      pad(date.getMinutes()) + ':' +

      pad(date.getSeconds());

  }

  

  static parseIso8601(str) {

    if (!str) return null;

    let m = str.match(/^(\d{4})(\d{2})(\d{2})T(\d{2}):(\d{2}):(\d{2})$/);

    if (m) {

      const [_, y, mo, d, h, mi, s] = m;

      return new Date(Number(y), Number(mo) - 1, Number(d), Number(h), Number(mi), Number(s));

    }

    m = str.match(/^(\d{4})-(\d{2})-(\d{2}) (\d{2}):(\d{2}):(\d{2})$/);

    if (m) {

      const [_, y, mo, d, h, mi, s] = m;

      return new Date(Number(y), Number(mo) - 1, Number(d), Number(h), Number(mi), Number(s));

    }

    const dt = new Date(str);

    return isNaN(dt.getTime()) ? null : dt;

  }

  

  static encodeValue(val) {

    if (val === null || val === undefined) return '<nil/>';

    if (Array.isArray(val)) return '<array><data>' + val.map(v => `<value>${XmlRpc.encodeValue(v)}</value>`).join('') + '</data></array>';

    const t = typeof val;

    if (t === 'boolean') return `<boolean>${val ? 1 : 0}</boolean>`;

    if (t === 'number') return Number.isInteger(val) ? `<int>${val}</int>` : `<double>${val}</double>`;

    if (val instanceof Date) return `<dateTime.iso8601>${XmlRpc.iso8601(val)}</dateTime.iso8601>`;

    if (t === 'object') return '<struct>' + Object.entries(val).map(([k, v]) =>

      `<member><name>${XmlRpc.escape(k)}</name><value>${XmlRpc.encodeValue(v)}</value></member>`).join('') + '</struct>';

    return `<string>${XmlRpc.escape(String(val))}</string>`;

  }

  

  static escape(s) {

    return s.replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;').replace(/"/g, '&quot;').replace(/'/g, '&#39;');

  }

  

  static buildCall(method, params) {

    const xml = `<?xml version="1.0"?><methodCall><methodName>${method}</methodName><params>` +

      params.map(p => `<param><value>${XmlRpc.encodeValue(p)}</value></param>`).join('') +

      `</params></methodCall>`;

    return xml;

  }

  

  static parseResponse(text) {

    const parseValue = (node) => {

      if (!node) return null;

      const nName = node.nodeName;

      if (nName === 'value') {

        if (node.children.length === 0) return node.textContent ?? '';

        return parseValue(node.children[0]);

      }

      if (['string','i4','int','double','boolean','dateTime.iso8601'].includes(nName)) {

        if (nName === 'boolean') return node.textContent.trim() === '1';

        if (nName === 'int' || nName === 'i4') return parseInt(node.textContent.trim(), 10);

        if (nName === 'double') return Number(node.textContent.trim());

        return node.textContent ?? '';

      }

      if (nName === 'struct') {

        const obj = {};

        for (let i = 0; i < node.children.length; i++) {

          const m = node.children[i];

          if (m.nodeName !== 'member') continue;

          const nameEl = m.getElementsByTagName('name')[0];

          const valEl = m.getElementsByTagName('value')[0];

          const key = nameEl ? nameEl.textContent : '';

          obj[key] = parseValue(valEl);

        }

        return obj;

      }

      if (nName === 'array') {

        const dataEl = node.getElementsByTagName('data')[0];

        if (!dataEl) return [];

        const vals = [];

        for (let i = 0; i < dataEl.children.length; i++) {

          if (dataEl.children[i].nodeName === 'value') vals.push(parseValue(dataEl.children[i]));

        }

        return vals;

      }

      return node.textContent ?? '';

    };

    const parser = new DOMParser();

    const doc = parser.parseFromString(text, "text/xml");

    const fault = doc.getElementsByTagName('fault')[0];

    if (fault) {

      const val = fault.getElementsByTagName('value')[0];

      const obj = parseValue(val);

      const msg = (obj && (obj.faultString || obj['faultString'])) || 'XML-RPC 错误';

      const code = (obj && (obj.faultCode || obj['faultCode'])) || -1;

      const err = new Error(`XML-RPC 错误 ${code}: ${msg}`);

      err.code = code;

      throw err;

    }

    const params = doc.getElementsByTagName('params')[0];

    if (!params) return null;

    const first = params.getElementsByTagName('param')[0];

    if (!first) return null;

    const value = first.getElementsByTagName('value')[0];

    return parseValue(value);

  }

}

  

class PublishModal extends Modal {

  constructor(app, preset, onSubmit, publishOffset = 0) {

    super(app);

    this.preset = preset || {};

    this.onSubmit = onSubmit;

    this.publishOffset = publishOffset;

  }

  

  onOpen() {

    const { contentEl } = this;

    contentEl.empty();

    contentEl.createEl('h2', { text: '发布到 Typecho' });

  

    this.values = {

      title: this.preset.title || '',

      slug: this.preset.slug || '',

      tags: (this.preset.tags || []).join(', '),

      categories: (this.preset.categories || []).join(', '),

      draft: this.preset.draft ?? false,

    };

  

    new Setting(contentEl).setName('标题').addText(t => t.setValue(this.values.title).onChange(v => this.values.title = v));

    new Setting(contentEl).setName('Slug').addText(t => t.setValue(this.values.slug).onChange(v => this.values.slug = v));

    new Setting(contentEl).setName('标签（逗号分隔）').addText(t => t.setValue(this.values.tags).onChange(v => this.values.tags = v));

    new Setting(contentEl).setName('分类（逗号分隔）').addText(t => t.setValue(this.values.categories).onChange(v => this.values.categories = v));

    new Setting(contentEl).setName('草稿').addToggle(t => t.setValue(this.values.draft).onChange(v => this.values.draft = v));

  

    if (this.publishOffset) {

      new Setting(contentEl).setName('发布时间偏移（小时）').setDesc(`当前偏移: ${this.publishOffset}`).addText(t => t.setValue(String(this.publishOffset)));

    }

  

    new Setting(contentEl)

      .addButton(b => b.setButtonText('发布').setCta().onClick(() => {

        if (!this.values.title) { new Notice('标题不能为空'); return; }

        if (!this.values.slug) { new Notice('Slug 不能为空'); return; }

        this.close();

        const payload = {

          title: this.values.title,

          slug: this.values.slug,

          tags: this.values.tags.split(',').map(s => s.trim()).filter(Boolean),

          categories: this.values.categories.split(',').map(s => s.trim()).filter(Boolean),

          draft: !!this.values.draft,

        };

        this.onSubmit(payload);

      }));

  }

}

  

module.exports = class TypechoXmlRpcPublisher extends Plugin {

  async onload() {

    this.settings = Object.assign({

      endpoint: '',

      username: '',

      password: '',

      defaultBlogId: '0',

      useFrontmatter: true,

      useCurrentTime: true,

      publishTimeOffset: 0,

      syncTimeOffset: 0,

      frontmatterKeys: { title:'title', slug:'slug', tags:'tags', categories:'categories', draft:'draft', cid:'cid', dateCreated:'dateCreated' }

    }, await this.loadData() || {});

  

    this.addSettingTab(new (class extends require('obsidian').PluginSettingTab {

      constructor(app, plugin){ super(app, plugin); this.plugin = plugin; }

      display() {

        const { containerEl } = this;

        containerEl.empty();

        containerEl.createEl('h2', { text: 'Typecho XML-RPC 发布器设置' });

  

        new Setting(containerEl).setName('接口 URL')

          .addText(t => t.setValue(this.plugin.settings.endpoint).onChange(async v => { this.plugin.settings.endpoint = v; await this.plugin.saveData(); }));

        new Setting(containerEl).setName('用户名')

          .addText(t => t.setValue(this.plugin.settings.username).onChange(async v => { this.plugin.settings.username = v; await this.plugin.saveData(); }));

        new Setting(containerEl).setName('密码')

          .addText(t => t.setValue(this.plugin.settings.password).onChange(async v => { this.plugin.settings.password = v; await this.plugin.saveData(); }).inputEl.setAttribute('type','password'));

        new Setting(containerEl).setName('默认博客ID')

          .addText(t => t.setValue(this.plugin.settings.defaultBlogId).onChange(async v => { this.plugin.settings.defaultBlogId = v; await this.plugin.saveData(); }));

        new Setting(containerEl).setName('总是使用当前时间作为发布时间')

          .addToggle(t => t.setValue(this.plugin.settings.useCurrentTime).onChange(async v => { this.plugin.settings.useCurrentTime = v; await this.plugin.saveData(); }));

  

        containerEl.createEl('h3', { text: '时间偏移设置' });

        new Setting(containerEl).setName('发布偏移（小时）').addText(t => t.setValue(String(this.plugin.settings.publishTimeOffset))

          .onChange(async v => { this.plugin.settings.publishTimeOffset = parseFloat(v) || 0; await this.plugin.saveData(); }));

        new Setting(containerEl).setName('同步偏移（小时）').addText(t => t.setValue(String(this.plugin.settings.syncTimeOffset))

          .onChange(async v => { this.plugin.settings.syncTimeOffset = parseFloat(v) || 0; await this.plugin.saveData(); }));

  

        containerEl.createEl('h3', { text: 'Frontmatter 对应键' });

        ['title','slug','tags','categories','draft','cid','dateCreated'].forEach(k => {

          new Setting(containerEl).setName(k + ' 键').addText(t => t.setValue(this.plugin.settings.frontmatterKeys[k])

            .onChange(async v => { this.plugin.settings.frontmatterKeys[k] = v; await this.plugin.saveData(); }));

        });

      }

    })(this.app, this));

  

    this.addCommand({

      id: 'typecho-publish-current-note',

      name: '发布当前笔记到 Typecho',

      callback: () => this.publishActiveNote(),

    });

  

    this.addCommand({

      id: 'typecho-sync-publish-date',

      name: '同步 Typecho 发布时间',

      callback: () => this.syncPublishDate(),

    });

  }

  

  async saveSettings() { await this.saveData(this.settings); }

  

  getActiveMDFile() {

    const file = this.app.workspace.getActiveFile();

    if (!file) throw new Error('未选中文件');

    if (file.extension !== 'md') throw new Error('当前文件不是 Markdown');

    return file;

  }

  

  async readFrontmatter(file) {

    const cache = this.app.metadataCache.getFileCache(file);

    return (cache && cache.frontmatter) || {};

  }

  

  async updateFrontmatter(file, updater) { await this.app.fileManager.processFrontMatter(file, updater); }

  

  extractContentWithoutFrontmatter(text) {

    if (text.startsWith('---')) {

      const end = text.indexOf('\n---', 3);

      if (end !== -1) return text.slice(end + 4).trimStart();

    }

    if (text.startsWith('+++')) {

      const end = text.indexOf('\n+++', 3);

      if (end !== -1) return text.slice(end + 4).trimStart();

    }

    return text;

  }

  

  async publishActiveNote() {

    try {

      const file = this.getActiveMDFile();

      const fm = await this.readFrontmatter(file);

      const raw = await this.app.vault.read(file);

      const body = this.extractContentWithoutFrontmatter(raw);

      const k = this.settings.frontmatterKeys;

      const preset = {

        title: fm[k.title] || file.basename,

        slug: fm[k.slug] || file.basename.toLowerCase().replace(/\s+/g, '-'),

        tags: Array.isArray(fm[k.tags]) ? fm[k.tags] : (typeof fm[k.tags] === 'string' ? fm[k.tags].split(',').map(s=>s.trim()).filter(Boolean) : []),

        categories: Array.isArray(fm[k.categories]) ? fm[k.categories] : (typeof fm[k.categories] === 'string' ? fm[k.categories].split(',').map(s=>s.trim()).filter(Boolean) : []),

        draft: !!fm[k.draft],

      };

  

      const proceed = async (vals) => {

        const postStruct = {

          title: vals.title,

          description: body,

          mt_keywords: vals.tags.join(','),

          categories: vals.categories,

          post_type: 'post',

          wp_slug: vals.slug,

          mt_allow_comments: 1,

        };

  

        let postDate = this.settings.useCurrentTime ? new Date() : (fm[k.dateCreated] ? XmlRpc.parseIso8601(fm[k.dateCreated]) || new Date() : new Date());

        if (this.settings.publishTimeOffset) postDate = new Date(postDate.getTime() + this.settings.publishTimeOffset*3600*1000);

        postStruct.dateCreated = postDate;

  

        const endpoint = this.settings.endpoint.trim();

        if (!endpoint) { new Notice('请设置接口 URL'); return; }

        const username = this.settings.username.trim();

        const password = this.settings.password;

        if (!username || !password) { new Notice('请填写用户名和密码'); return; }

  

        const cidKey = k.cid;

        const existingCid = fm[cidKey];

        let method, params;

        if (existingCid) { method='metaWeblog.editPost'; params=[String(existingCid), username, password, postStruct, !vals.draft]; }

        else { method='metaWeblog.newPost'; params=[String(this.settings.defaultBlogId || '0'), username, password, postStruct, !vals.draft]; }

  

        const xml = XmlRpc.buildCall(method, params);

        const response = await requestUrl({ url:endpoint, method:'POST', headers:{'Content-Type':'text/xml'}, body:xml });

        const resultValue = XmlRpc.parseResponse(response.text);

  

        await this.updateFrontmatter(file, fm => {

          fm[k.title] = vals.title; fm[k.slug] = vals.slug; fm[k.tags]=vals.tags; fm[k.categories]=vals.categories; fm[k.draft]=vals.draft;

          fm[k.dateCreated]=XmlRpc.iso8601(postStruct.dateCreated); fm['lastPublished']=XmlRpc.iso8601(new Date());

          if (!existingCid) fm[cidKey]=String(resultValue);

        });

  

        new Notice(existingCid ? `更新文章成功 (cid=${existingCid})` : `发布新文章成功 (cid=${String(resultValue)})`);

      };

  

      new PublishModal(this.app, preset, proceed, this.settings.publishTimeOffset).open();

    } catch(e) { console.error(e); new Notice('错误: '+e.message); }

  }

  

  async syncPublishDate() {

    try {

      const file = this.getActiveMDFile();

      const fm = await this.readFrontmatter(file);

      const k = this.settings.frontmatterKeys;

      const cid = fm[k.cid];

      if (!cid) { new Notice('未找到文章 CID'); return; }

  

      const xml = XmlRpc.buildCall('metaWeblog.getPost', [ String(cid), this.settings.username, this.settings.password ]);

      const response = await requestUrl({ url:this.settings.endpoint, method:'POST', headers:{'Content-Type':'text/xml'}, body:xml });

      const result = XmlRpc.parseResponse(response.text);

      if (!result || !result.dateCreated) { new Notice('获取发布时间失败'); return; }

  

      let serverDate = XmlRpc.parseIso8601(result.dateCreated);

      if (this.settings.syncTimeOffset) serverDate = new Date(serverDate.getTime() + this.settings.syncTimeOffset*3600*1000);

      await this.updateFrontmatter(file, fm => { fm[k.dateCreated] = XmlRpc.iso8601(serverDate); });

  

      new Notice('发布时间已同步');

    } catch(e) { console.error(e); new Notice('错误: '+e.message); }

  }

};
```

下面是manifest.json文件代码：
```json
{
  "id": "typecho-xmlrpc-publisher",
  "name": "Typecho XML-RPC Publisher",
  "version": "1.1.0",
  "minAppVersion": "1.5.0",
  "description": "通过 XML-RPC （MetaWeblog） 将当前笔记发布到 Typecho 博客。支持标题、slug、标签、类别、草稿；可选是否更新发布时间；支持强制同步服务器发布时间。",
  "author": "Hans J. Han",
  "authorUrl": "https://www.hansjack.com",
  "isDesktopOnly": false,
  "js": "main.js"
}
```


### 3、版本1.2：添加文章管理、添加发布日历时间选择

![image.png](https://cdn.hansjack.com/2025/08/f399cd21f5b176780bf03346199f4e25.webp)

添加日历时间：
![image.png](https://cdn.hansjack.com/2025/08/8e0bcf6a3d568b6705b78f7ecffb6f08.webp)


```javascript
// Typecho XML-RPC 发布器 - main.js

const { Plugin, Modal, Setting, Notice, requestUrl } = require('obsidian');

  

class XmlRpc {

  static iso8601(date) {

    const pad = (n) => (n < 10 ? '0' + n : '' + n);

    return date.getFullYear().toString() +

      pad(date.getMonth() + 1) +

      pad(date.getDate()) + 'T' +

      pad(date.getHours()) + ':' +

      pad(date.getMinutes()) + ':' +

      pad(date.getSeconds());

  }

  

  static parseIso8601(str) {

    if (!str) return null;

    let m = str.match(/^(\d{4})(\d{2})(\d{2})T(\d{2}):(\d{2}):(\d{2})$/);

    if (m) {

      const [_, y, mo, d, h, mi, s] = m;

      return new Date(Number(y), Number(mo) - 1, Number(d), Number(h), Number(mi), Number(s));

    }

    m = str.match(/^(\d{4})-(\d{2})-(\d{2}) (\d{2}):(\d{2}):(\d{2})$/);

    if (m) {

      const [_, y, mo, d, h, mi, s] = m;

      return new Date(Number(y), Number(mo) - 1, Number(d), Number(h), Number(mi), Number(s));

    }

    const dt = new Date(str);

    return isNaN(dt.getTime()) ? null : dt;

  }

  

  static encodeValue(val) {

    if (val === null || val === undefined) return '<nil/>';

    if (Array.isArray(val)) return '<array><data>' + val.map(v => `<value>${XmlRpc.encodeValue(v)}</value>`).join('') + '</data></array>';

    const t = typeof val;

    if (t === 'boolean') return `<boolean>${val ? 1 : 0}</boolean>`;

    if (t === 'number') return Number.isInteger(val) ? `<int>${val}</int>` : `<double>${val}</double>`;

    if (val instanceof Date) return `<dateTime.iso8601>${XmlRpc.iso8601(val)}</dateTime.iso8601>`;

    if (t === 'object') return '<struct>' + Object.entries(val).map(([k, v]) =>

      `<member><name>${XmlRpc.escape(k)}</name><value>${XmlRpc.encodeValue(v)}</value></member>`).join('') + '</struct>';

    return `<string>${XmlRpc.escape(String(val))}</string>`;

  }

  

  static escape(s) {

    return s.replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;').replace(/"/g, '&quot;').replace(/'/g, '&#39;');

  }

  

  static buildCall(method, params) {

    const xml = `<?xml version="1.0"?><methodCall><methodName>${method}</methodName><params>` +

      params.map(p => `<param><value>${XmlRpc.encodeValue(p)}</value></param>`).join('') +

      `</params></methodCall>`;

    return xml;

  }

  

  static parseResponse(text) {

    const parseValue = (node) => {

      if (!node) return null;

      const nName = node.nodeName;

      if (nName === 'value') {

        if (node.children.length === 0) return node.textContent ?? '';

        return parseValue(node.children[0]);

      }

      if (['string','i4','int','double','boolean','dateTime.iso8601'].includes(nName)) {

        if (nName === 'boolean') return node.textContent.trim() === '1';

        if (nName === 'int' || nName === 'i4') return parseInt(node.textContent.trim(), 10);

        if (nName === 'double') return Number(node.textContent.trim());

        return node.textContent ?? '';

      }

      if (nName === 'struct') {

        const obj = {};

        for (let i = 0; i < node.children.length; i++) {

          const m = node.children[i];

          if (m.nodeName !== 'member') continue;

          const nameEl = m.getElementsByTagName('name')[0];

          const valEl = m.getElementsByTagName('value')[0];

          const key = nameEl ? nameEl.textContent : '';

          obj[key] = parseValue(valEl);

        }

        return obj;

      }

      if (nName === 'array') {

        const dataEl = node.getElementsByTagName('data')[0];

        if (!dataEl) return [];

        const vals = [];

        for (let i = 0; i < dataEl.children.length; i++) {

          if (dataEl.children[i].nodeName === 'value') vals.push(parseValue(dataEl.children[i]));

        }

        return vals;

      }

      return node.textContent ?? '';

    };

    const parser = new DOMParser();

    const doc = parser.parseFromString(text, "text/xml");

    const fault = doc.getElementsByTagName('fault')[0];

    if (fault) {

      const val = fault.getElementsByTagName('value')[0];

      const obj = parseValue(val);

      const msg = (obj && (obj.faultString || obj['faultString'])) || 'XML-RPC 错误';

      const code = (obj && (obj.faultCode || obj['faultCode'])) || -1;

      const err = new Error(`XML-RPC 错误 ${code}: ${msg}`);

      err.code = code;

      throw err;

    }

    const params = doc.getElementsByTagName('params')[0];

    if (!params) return null;

    const first = params.getElementsByTagName('param')[0];

    if (!first) return null;

    const value = first.getElementsByTagName('value')[0];

    return parseValue(value);

  }

}

  
  

class ManageModal extends Modal {

  constructor(app, plugin) {

    super(app);

    this.plugin = plugin;

  }

  

  async onOpen() {

    const { contentEl } = this;

    contentEl.empty();

    contentEl.createEl('h2', { text: '文章管理' });

  

    try {

      const xml = XmlRpc.buildCall('metaWeblog.getRecentPosts', [

        String(this.plugin.settings.defaultBlogId || '0'),

        this.plugin.settings.username,

        this.plugin.settings.password,

        this.plugin.settings.managePostsCount || 20

      ]);

      const resp = await requestUrl({

        url:this.plugin.settings.endpoint,

        method:'POST',

        headers:{'Content-Type':'text/xml'},

        body:xml

      });

      const posts = XmlRpc.parseResponse(resp.text) || [];

  

      posts.forEach(post => {

        const card = contentEl.createEl('div', { cls: 'post-card' });

  

        const header = card.createEl('div', { cls: 'post-header' });

        const titleEl = header.createEl('a', { text: post.title, href: post.link, cls: 'post-title' });

        titleEl.setAttr('target','_blank');

  

        const actions = header.createEl('div', { cls:'post-actions' });

        const btnDelete = actions.createEl('button', { text: '删除', cls:'action-btn delete' });

        btnDelete.onclick = async () => {

          const delXml = XmlRpc.buildCall('blogger.deletePost', [

            '0', String(post.postid), this.plugin.settings.username, this.plugin.settings.password, true

          ]);

          await requestUrl({ url:this.plugin.settings.endpoint, method:'POST', headers:{'Content-Type':'text/xml'}, body:delXml });

          new Notice('文章已删除');

          this.onOpen();

        };

  

        const btnImport = actions.createEl('button', { text: '导入到 Obsidian', cls:'action-btn import' });

        btnImport.onclick = async () => {

          const fileName = `${post.title}.md`;

          const tagsArray = (post.mt_keywords || '').split(',').map(s => s.trim()).filter(Boolean);

          const categoriesArray = Array.isArray(post.categories) ? post.categories : [];

const yamlLines = [

  '---',

  `title: ${post.title}`,

  `slug: "${post.wp_slug || ''}"`,        // 用双引号包裹，保证是文本

  'tags:',

  ...tagsArray.map(t => `  - ${t}`),

  'categories:',

  ...categoriesArray.map(c => `  - ${c}`),

  `draft: ${post.post_status === 'draft'}`,

  `cid: "${post.postid}"`,                // 用双引号包裹，保证是文本

  `dateCreated: ${XmlRpc.iso8601(XmlRpc.parseIso8601(post.dateCreated))}`,

  '---',

  '',

].join('\n');

  
  

          const fileContent = `${yamlLines}\n${post.description || ''}`;

          await this.plugin.app.vault.create(fileName, fileContent);

          new Notice(`已导入文章: ${fileName}`);

        };

  

        card.createEl('div', { cls:'post-meta', text: `分类: ${(post.categories||[]).join(', ')} | 标签: ${post.mt_keywords||''}` });

      });

  

    } catch(e) {

      console.error(e);

      new Notice('获取文章失败: '+e.message);

    }

  }

}

  
  

class PublishModal extends Modal {

  constructor(app, preset, onSubmit, publishOffset = 0, useCurrentTime = true) {

    super(app);

    this.preset = preset || {};

    this.onSubmit = onSubmit;

    this.publishOffset = publishOffset;

    this.useCurrentTime = useCurrentTime;

  }

  

  onOpen() {

    const { contentEl } = this;

    contentEl.empty();

    contentEl.createEl('h2', { text: '发布到 Typecho' });

  

    // 初始化时间

    let initDate;

    if (this.useCurrentTime) {

      initDate = new Date();

    } else {

      initDate = this.preset.date ? new Date(this.preset.date) : new Date();

    }

  

    this.values = {

      title: this.preset.title || '',

      slug: this.preset.slug || '',

      tags: (this.preset.tags || []).join(', '),

      categories: (this.preset.categories || []).join(', '),

      draft: this.preset.draft ?? false,

      date: initDate,

    };

  

    const pad = n => n.toString().padStart(2, '0');

    const toLocalDatetime = d => `${d.getFullYear()}-${pad(d.getMonth()+1)}-${pad(d.getDate())}T${pad(d.getHours())}:${pad(d.getMinutes())}`;

  

    new Setting(contentEl)

      .setName('标题')

      .setDesc('文章标题，将显示在 Typecho 前端')

      .addText(t => t.setValue(this.values.title).onChange(v => this.values.title = v));

  

    new Setting(contentEl)

      .setName('Slug')

      .setDesc('文章 URL 片段（如不填写将自动生成）')

      .addText(t => t.setValue(this.values.slug).onChange(v => this.values.slug = v));

  

    new Setting(contentEl)

      .setName('标签（逗号分隔）')

      .setDesc('多个标签用英文逗号分隔')

      .addText(t => t.setValue(this.values.tags).onChange(v => this.values.tags = v));

  

    new Setting(contentEl)

      .setName('分类（逗号分隔）')

      .setDesc('文章分类，用英文逗号分隔')

      .addText(t => t.setValue(this.values.categories).onChange(v => this.values.categories = v));

  

    new Setting(contentEl)

      .setName('草稿')

      .setDesc('启用后文章不会立即公开')

      .addToggle(t => t.setValue(this.values.draft).onChange(v => this.values.draft = v));

  

    new Setting(contentEl)

      .setName('发布时间')

      .setDesc(`选择文章发布时间（当前偏移: ${this.publishOffset} 小时）`)

      .addText(t => t

        .setValue(toLocalDatetime(this.values.date))

        .onChange(v => {

          this.values.date = new Date(v);

        })

        .inputEl.setAttribute('type', 'datetime-local'));

  

    new Setting(contentEl)

      .addButton(b => b.setButtonText(' 发布 ').setCta().onClick(() => {

        if (!this.values.title) { new Notice('标题不能为空'); return; }

        if (!this.values.slug) { new Notice('Slug 不能为空'); return; }

        this.close();

        const payload = {

          title: this.values.title,

          slug: this.values.slug,

          tags: this.values.tags.split(',').map(s => s.trim()).filter(Boolean),

          categories: this.values.categories.split(',').map(s => s.trim()).filter(Boolean),

          draft: !!this.values.draft,

          dateCreated: this.values.date,

        };

        this.onSubmit(payload);

      }));

  }

}

  
  
  

module.exports = class TypechoXmlRpcPublisher extends Plugin {

  async onload() {

    this.settings = Object.assign({

      endpoint: '',

      username: '',

      password: '',

      defaultBlogId: '0',

      useFrontmatter: true,

      useCurrentTime: true,

      publishTimeOffset: 0,

      syncTimeOffset: 0,

      managePostsCount: 20,

      frontmatterKeys: { title:'title', slug:'slug', tags:'tags', categories:'categories', draft:'draft', cid:'cid', dateCreated:'dateCreated' }

    }, await this.loadData() || {});

  

    this.addSettingTab(new (class extends require('obsidian').PluginSettingTab {

      constructor(app, plugin){ super(app, plugin); this.plugin = plugin; }

      display() {

        const { containerEl } = this;

        containerEl.empty();

        containerEl.createEl('h2', { text: 'Typecho XML-RPC 发布器设置' });

  

        const settingsInputs = {};

  

        settingsInputs.endpoint = new Setting(containerEl).setName('接口 URL')

          .setDesc('填写 Typecho XML-RPC 接口地址，例如：https://your-site.com/xmlrpc.php')

          .addText(t => t.setValue(this.plugin.settings.endpoint).onChange(v => settingsInputs.endpoint.value = v));

  

        settingsInputs.username = new Setting(containerEl).setName('用户名')

          .setDesc('用于登录 Typecho 的用户名，确保有发布权限')

          .addText(t => t.setValue(this.plugin.settings.username).onChange(v => settingsInputs.username.value = v));

  

        settingsInputs.password = new Setting(containerEl).setName('密码')

          .setDesc('对应用户名的密码，支持明文输入（将以安全方式保存）')

          .addText(t => t.setValue(this.plugin.settings.password).onChange(v => settingsInputs.password.value = v).inputEl.setAttribute('type','password'));

  

        settingsInputs.defaultBlogId = new Setting(containerEl).setName('默认博客ID')

          .setDesc('如果你的 Typecho 有多个博客，设置默认发布的博客 ID')

          .addText(t => t.setValue(this.plugin.settings.defaultBlogId).onChange(v => settingsInputs.defaultBlogId.value = v));

  

        settingsInputs.useCurrentTime = new Setting(containerEl).setName('总是使用当前时间作为发布时间')

          .setDesc('启用后发布文章时会忽略文章原有 Frontmatter 时间，直接使用当前时间')

          .addToggle(t => t.setValue(this.plugin.settings.useCurrentTime).onChange(v => settingsInputs.useCurrentTime.value = v));

  

        containerEl.createEl('h3', { text: '时间偏移设置' });

  

        settingsInputs.publishTimeOffset = new Setting(containerEl).setName('发布偏移（小时）')

          .setDesc('调整发布到 Typecho 的时间，例如 +8 表示服务器时间加 8 小时')

          .addText(t => t.setValue(String(this.plugin.settings.publishTimeOffset)).onChange(v => settingsInputs.publishTimeOffset.value = v));

  

        settingsInputs.syncTimeOffset = new Setting(containerEl).setName('同步偏移（小时）')

          .setDesc('同步 Typecho 发布时间时的时间偏移设置')

          .addText(t => t.setValue(String(this.plugin.settings.syncTimeOffset)).onChange(v => settingsInputs.syncTimeOffset.value = v));

  

        containerEl.createEl('h3', { text: '文章管理设置' });

  

        settingsInputs.managePostsCount = new Setting(containerEl).setName('文章管理显示数量') // 添加文章管理显示数量设置

          .setDesc('在文章管理界面显示的文章数量')

          .addText(t => t.setValue(String(this.plugin.settings.managePostsCount)).onChange(v => settingsInputs.managePostsCount.value = v));

  
  

        containerEl.createEl('h3', { text: 'Frontmatter 对应键' });

  

        const frontmatterKeys = {};

        ['title','slug','tags','categories','draft','cid','dateCreated'].forEach(k => {

          frontmatterKeys[k] = new Setting(containerEl).setName(k + ' 键')

            .setDesc(`对应 Frontmatter 中的 ${k} 键，用于自动读取或写入`)

            .addText(t => t.setValue(this.plugin.settings.frontmatterKeys[k]).onChange(v => frontmatterKeys[k].value = v));

        });

  

        new Setting(containerEl)

          .addButton(b => b.setButtonText('保存设置').setCta().onClick(async () => {

            this.plugin.settings.endpoint = settingsInputs.endpoint.value ?? this.plugin.settings.endpoint;

            this.plugin.settings.username = settingsInputs.username.value ?? this.plugin.settings.username;

            this.plugin.settings.password = settingsInputs.password.value ?? this.plugin.settings.password;

            this.plugin.settings.defaultBlogId = settingsInputs.defaultBlogId.value ?? this.plugin.settings.defaultBlogId;

            this.plugin.settings.useCurrentTime = settingsInputs.useCurrentTime.value ?? this.plugin.settings.useCurrentTime;

            this.plugin.settings.publishTimeOffset = parseFloat(settingsInputs.publishTimeOffset.value) || 0;

            this.plugin.settings.syncTimeOffset = parseFloat(settingsInputs.syncTimeOffset.value) || 0;

            this.plugin.settings.managePostsCount = parseInt(settingsInputs.managePostsCount.value) || 20;

  

            for (let k of ['title','slug','tags','categories','draft','cid','dateCreated']) {

              this.plugin.settings.frontmatterKeys[k] = frontmatterKeys[k].value ?? this.plugin.settings.frontmatterKeys[k];

            }

  

            await this.plugin.saveData(this.plugin.settings);

            new Notice('设置已保存');

          }));

      }

    })(this.app, this));

  

    this.addCommand({

      id: 'typecho-publish-current-note',

      name: '发布当前笔记到 Typecho',

      callback: () => this.publishActiveNote(),

    });

  

    this.addCommand({

      id: 'typecho-sync-publish-date',

      name: '同步 Typecho 发布时间',

      callback: () => this.syncPublishDate(),

    });

  

    this.addCommand({

      id: 'typecho-manage-posts',

      name: '管理文章',

      callback: () => new ManageModal(this.app, this).open(),

    });

  }

  

  async getActiveMDFile() {

    const file = this.app.workspace.getActiveFile();

    if (!file) throw new Error('未选中文件');

    if (file.extension !== 'md') throw new Error('当前文件不是 Markdown');

    return file;

  }

  

  async readFrontmatter(file) {

    const cache = this.app.metadataCache.getFileCache(file);

    return (cache && cache.frontmatter) || {};

  }

  

  async updateFrontmatter(file, updater) { await this.app.fileManager.processFrontMatter(file, updater); }

  

  extractContentWithoutFrontmatter(text) {

    if (text.startsWith('---')) {

      const end = text.indexOf('\n---', 3);

      if (end !== -1) return text.slice(end + 4).trimStart();

    }

    if (text.startsWith('+++')) {

      const end = text.indexOf('\n+++', 3);

      if (end !== -1) return text.slice(end + 4).trimStart();

    }

    return text;

  }

  

async publishActiveNote() {

  try {

    const file = await this.getActiveMDFile();

    const fm = await this.readFrontmatter(file);

    const raw = await this.app.vault.read(file);

    const body = this.extractContentWithoutFrontmatter(raw);

    const k = this.settings.frontmatterKeys;

    const preset = {

      title: fm[k.title] || file.basename,

      slug: fm[k.slug] || file.basename.toLowerCase().replace(/\s+/g, '-'),

      tags: Array.isArray(fm[k.tags]) ? fm[k.tags] : (typeof fm[k.tags] === 'string' ? fm[k.tags].split(',').map(s=>s.trim()).filter(Boolean) : []),

      categories: Array.isArray(fm[k.categories]) ? fm[k.categories] : (typeof fm[k.categories] === 'string' ? fm[k.categories].split(',').map(s=>s.trim()).filter(Boolean) : []),

      draft: !!fm[k.draft],

      date: fm[k.dateCreated] ? XmlRpc.parseIso8601(fm[k.dateCreated]) : new Date(),

    };

  

    const proceed = async (vals) => {

      const postStruct = {

        title: vals.title,

        description: body,

        mt_keywords: vals.tags.join(','),

        categories: vals.categories,

        post_type: 'post',

        wp_slug: vals.slug,

        mt_allow_comments: 1,

      };

  

      // 如果设置总是使用当前时间，覆盖用户选择

      let postDate = this.settings.useCurrentTime ? new Date() : (vals.dateCreated || new Date());

      if (this.settings.publishTimeOffset) {

        postDate = new Date(postDate.getTime() + this.settings.publishTimeOffset * 3600 * 1000);

      }

      postStruct.dateCreated = postDate;

  

      const endpoint = this.settings.endpoint.trim();

      if (!endpoint) { new Notice('请设置接口 URL'); return; }

      const username = this.settings.username.trim();

      const password = this.settings.password;

      if (!username || !password) { new Notice('请填写用户名和密码'); return; }

  

      const cidKey = k.cid;

      const existingCid = fm[cidKey];

      let method, params;

      if (existingCid) { method='metaWeblog.editPost'; params=[String(existingCid), username, password, postStruct, !vals.draft]; }

      else { method='metaWeblog.newPost'; params=[String(this.settings.defaultBlogId || '0'), username, password, postStruct, !vals.draft]; }

  

      const xml = XmlRpc.buildCall(method, params);

      const response = await requestUrl({ url:endpoint, method:'POST', headers:{'Content-Type':'text/xml'}, body:xml });

      const resultValue = XmlRpc.parseResponse(response.text);

  

      await this.updateFrontmatter(file, fm => {

        fm[k.title] = vals.title;

        fm[k.slug] = vals.slug;

        fm[k.tags] = vals.tags;

        fm[k.categories] = vals.categories;

        fm[k.draft] = vals.draft;

        fm[k.dateCreated] = XmlRpc.iso8601(postStruct.dateCreated);

        fm['lastPublished'] = XmlRpc.iso8601(new Date());

        if (!existingCid) fm[cidKey] = String(resultValue);

      });

  

      new Notice(existingCid ? `更新文章成功 (cid=${existingCid})` : `发布新文章成功 (cid=${String(resultValue)})`);

    };

  

    new PublishModal(this.app, preset, proceed, this.settings.publishTimeOffset, this.settings.useCurrentTime).open();

  } catch(e) { console.error(e); new Notice('错误: '+e.message); }

}

  

  async syncPublishDate() {

    try {

      const file = await this.getActiveMDFile();

      const fm = await this.readFrontmatter(file);

      const k = this.settings.frontmatterKeys;

      const cid = fm[k.cid];

      if (!cid) { new Notice('未找到文章 CID'); return; }

  

      const xml = XmlRpc.buildCall('metaWeblog.getPost', [ String(cid), this.settings.username, this.settings.password ]);

      const response = await requestUrl({ url:this.settings.endpoint, method:'POST', headers:{'Content-Type':'text/xml'}, body:xml });

      const result = XmlRpc.parseResponse(response.text);

      if (!result || !result.dateCreated) { new Notice('获取发布时间失败'); return; }

  

      let serverDate = XmlRpc.parseIso8601(result.dateCreated);

      if (this.settings.syncTimeOffset) serverDate = new Date(serverDate.getTime() + this.settings.syncTimeOffset*3600*1000);

      await this.updateFrontmatter(file, fm => { fm[k.dateCreated] = XmlRpc.iso8601(serverDate); });

  

      new Notice('发布时间已同步');

    } catch(e) { console.error(e); new Notice('错误: '+e.message); }

  }

};
```

添加mainifest.json文件在插件里面：

```json
{
  "id": "typecho-xmlrpc-publisher",
  "name": "Typecho XML-RPC Publisher",
  "version": "1.2.0",
  "minAppVersion": "1.5.0",
  "description": "通过 XML-RPC （MetaWeblog） 将当前笔记发布到 Typecho 博客。支持标题、slug、标签、类别、草稿；可选是否更新发布时间；支持强制同步服务器发布时间。",
  "author": "Hans J. Han",
  "authorUrl": "https://www.hansjack.com",
  "isDesktopOnly": false,
  "js": "main.js"
}
```


### 4、版本1.3：添加调试信息、修复分类空白问题

![bab967002125e0b470e705ee6656a493.png](https://cdn.hansjack.com/2025/08/2a02fb6ebff9a060278710ee34a02c94.webp)
![image.png](https://cdn.hansjack.com/2025/08/35f499b5995c6d734fe966abaa4e2661.webp)


> 更新说明：直接替换main.js文件代码（必须），修改mainifest.json中版本信息为1.3（可选）

```javascript
// Typecho XML-RPC 发布器 - main.js

const { Plugin, Modal, Setting, Notice, requestUrl } = require('obsidian');

  

class XmlRpc {

  static iso8601(date) {

    const pad = (n) => (n < 10 ? '0' + n : '' + n);

    return date.getFullYear().toString() +

      pad(date.getMonth() + 1) +

      pad(date.getDate()) + 'T' +

      pad(date.getHours()) + ':' +

      pad(date.getMinutes()) + ':' +

      pad(date.getSeconds());

  }

  

  static parseIso8601(str) {

    if (!str) return null;

    let m = str.match(/^(\d{4})(\d{2})(\d{2})T(\d{2}):(\d{2}):(\d{2})$/);

    if (m) {

      const [_, y, mo, d, h, mi, s] = m;

      return new Date(Number(y), Number(mo) - 1, Number(d), Number(h), Number(mi), Number(s));

    }

    m = str.match(/^(\d{4})-(\d{2})-(\d{2}) (\d{2}):(\d{2}):(\d{2})$/);

    if (m) {

      const [_, y, mo, d, h, mi, s] = m;

      return new Date(Number(y), Number(mo) - 1, Number(d), Number(h), Number(mi), Number(s));

    }

    const dt = new Date(str);

    return isNaN(dt.getTime()) ? null : dt;

  }

  

  static encodeValue(val) {

    if (val === null || val === undefined) return '<nil/>';

    if (Array.isArray(val)) return '<array><data>' + val.map(v => `<value>${XmlRpc.encodeValue(v)}</value>`).join('') + '</data></array>';

    const t = typeof val;

    if (t === 'boolean') return `<boolean>${val ? 1 : 0}</boolean>`;

    if (t === 'number') return Number.isInteger(val) ? `<int>${val}</int>` : `<double>${val}</double>`;

    if (val instanceof Date) return `<dateTime.iso8601>${XmlRpc.iso8601(val)}</dateTime.iso8601>`;

    if (t === 'object') return '<struct>' + Object.entries(val).map(([k, v]) =>

      `<member><name>${XmlRpc.escape(k)}</name><value>${XmlRpc.encodeValue(v)}</value></member>`).join('') + '</struct>';

    return `<string>${XmlRpc.escape(String(val))}</string>`;

  }

  

  static escape(s) {

    return s.replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;').replace(/"/g, '&quot;').replace(/'/g, '&#39;');

  }

  

  static buildCall(method, params) {

    const xml = `<?xml version="1.0"?><methodCall><methodName>${method}</methodName><params>` +

      params.map(p => `<param><value>${XmlRpc.encodeValue(p)}</value></param>`).join('') +

      `</params></methodCall>`;

    return xml;

  }

  

  static parseResponse(text) {

    const parseValue = (node) => {

      if (!node) return null;

      const nName = node.nodeName;

      if (nName === 'value') {

        if (node.children.length === 0) return node.textContent ?? '';

        return parseValue(node.children[0]);

      }

      if (['string','i4','int','double','boolean','dateTime.iso8601'].includes(nName)) {

        if (nName === 'boolean') return node.textContent.trim() === '1';

        if (nName === 'int' || nName === 'i4') return parseInt(node.textContent.trim(), 10);

        if (nName === 'double') return Number(node.textContent.trim());

        return node.textContent ?? '';

      }

      if (nName === 'struct') {

        const obj = {};

        for (let i = 0; i < node.children.length; i++) {

          const m = node.children[i];

          if (m.nodeName !== 'member') continue;

          const nameEl = m.getElementsByTagName('name')[0];

          const valEl = m.getElementsByTagName('value')[0];

          const key = nameEl ? nameEl.textContent : '';

          obj[key] = parseValue(valEl);

        }

        return obj;

      }

      if (nName === 'array') {

        const dataEl = node.getElementsByTagName('data')[0];

        if (!dataEl) return [];

        const vals = [];

        for (let i = 0; i < dataEl.children.length; i++) {

          if (dataEl.children[i].nodeName === 'value') vals.push(parseValue(dataEl.children[i]));

        }

        return vals;

      }

      return node.textContent ?? '';

    };

    const parser = new DOMParser();

    const doc = parser.parseFromString(text, "text/xml");

    const fault = doc.getElementsByTagName('fault')[0];

    if (fault) {

      const val = fault.getElementsByTagName('value')[0];

      const obj = parseValue(val);

      const msg = (obj && (obj.faultString || obj['faultString'])) || 'XML-RPC 错误';

      const code = (obj && (obj.faultCode || obj['faultCode'])) || -1;

      const err = new Error(`XML-RPC 错误 ${code}: ${msg}`);

      err.code = code;

      throw err;

    }

    const params = doc.getElementsByTagName('params')[0];

    if (!params) return null;

    const first = params.getElementsByTagName('param')[0];

    if (!first) return null;

    const value = first.getElementsByTagName('value')[0];

    return parseValue(value);

  }

}

  
  

class ManageModal extends Modal {

  constructor(app, plugin) {

    super(app);

    this.plugin = plugin;

  }

  

  async onOpen() {

    const { contentEl } = this;

    contentEl.empty();

    contentEl.createEl('h2', { text: '文章管理' });

  

    try {

      const xml = XmlRpc.buildCall('metaWeblog.getRecentPosts', [

        String(this.plugin.settings.defaultBlogId || '0'),

        this.plugin.settings.username,

        this.plugin.settings.password,

        this.plugin.settings.managePostsCount || 20

      ]);

      const resp = await requestUrl({

        url:this.plugin.settings.endpoint,

        method:'POST',

        headers:{'Content-Type':'text/xml'},

        body:xml

      });

      const posts = XmlRpc.parseResponse(resp.text) || [];

  

      posts.forEach(post => {

        const card = contentEl.createEl('div', { cls: 'post-card' });

  

        const header = card.createEl('div', { cls: 'post-header' });

        const titleEl = header.createEl('a', { text: post.title, href: post.link, cls: 'post-title' });

        titleEl.setAttr('target','_blank');

  

        const actions = header.createEl('div', { cls:'post-actions' });

        const btnDelete = actions.createEl('button', { text: '删除', cls:'action-btn delete' });

        btnDelete.onclick = async () => {

          const delXml = XmlRpc.buildCall('blogger.deletePost', [

            '0', String(post.postid), this.plugin.settings.username, this.plugin.settings.password, true

          ]);

          await requestUrl({ url:this.plugin.settings.endpoint, method:'POST', headers:{'Content-Type':'text/xml'}, body:delXml });

          new Notice('文章已删除');

          this.onOpen();

        };

  

        const btnImport = actions.createEl('button', { text: '导入到 Obsidian', cls:'action-btn import' });

        btnImport.onclick = async () => {

          const fileName = `${post.title}.md`;

          const tagsArray = (post.mt_keywords || '').split(',').map(s => s.trim()).filter(Boolean);

          const categoriesArray = Array.isArray(post.categories) ? post.categories : [];

const yamlLines = [

  '---',

  `title: ${post.title}`,

  `slug: "${post.wp_slug || ''}"`,        // 用双引号包裹，保证是文本

  'tags:',

  ...tagsArray.map(t => `  - ${t}`),

  'categories:',

  ...categoriesArray.map(c => `  - ${c}`),

  `draft: ${post.post_status === 'draft'}`,

  `cid: "${post.postid}"`,                // 用双引号包裹，保证是文本

  `dateCreated: ${XmlRpc.iso8601(XmlRpc.parseIso8601(post.dateCreated))}`,

  '---',

  '',

].join('\n');

  
  

          const fileContent = `${yamlLines}\n${post.description || ''}`;

          await this.plugin.app.vault.create(fileName, fileContent);

          new Notice(`已导入文章: ${fileName}`);

        };

  

        card.createEl('div', { cls:'post-meta', text: `分类: ${(post.categories||[]).join(', ')} | 标签: ${post.mt_keywords||''}` });

      });

  

    } catch(e) {

      console.error(e);

      new Notice('获取文章失败: '+e.message);

    }

  }

}

  
  

class PublishModal extends Modal {

  constructor(app, preset, onSubmit, publishOffset = 0, useCurrentTime = true) {

    super(app);

    this.preset = preset || {};

    this.onSubmit = onSubmit;

    this.publishOffset = publishOffset;

    this.useCurrentTime = useCurrentTime;

  }

  

  onOpen() {

    const { contentEl } = this;

    contentEl.empty();

    contentEl.createEl('h2', { text: '发布到 Typecho' });

  

    // 初始化时间

    let initDate;

    if (this.useCurrentTime) {

      initDate = new Date();

    } else {

      initDate = this.preset.date ? new Date(this.preset.date) : new Date();

    }

  

    this.values = {

      title: this.preset.title || '',

      slug: this.preset.slug || '',

      tags: (this.preset.tags || []).join(', '),

      categories: (this.preset.categories || []).join(', '),

      draft: this.preset.draft ?? false,

      date: initDate,

    };

  

    const pad = n => n.toString().padStart(2, '0');

    const toLocalDatetime = d => `${d.getFullYear()}-${pad(d.getMonth()+1)}-${pad(d.getDate())}T${pad(d.getHours())}:${pad(d.getMinutes())}`;

  

    new Setting(contentEl)

      .setName('标题')

      .setDesc('文章标题，将显示在 Typecho 前端')

      .addText(t => t.setValue(this.values.title).onChange(v => this.values.title = v));

  

    new Setting(contentEl)

      .setName('Slug')

      .setDesc('文章 URL 片段（如不填写将自动生成）')

      .addText(t => t.setValue(this.values.slug).onChange(v => this.values.slug = v));

  

    new Setting(contentEl)

      .setName('标签（逗号分隔）')

      .setDesc('多个标签用英文逗号分隔')

      .addText(t => t.setValue(this.values.tags).onChange(v => this.values.tags = v));

  

    new Setting(contentEl)

      .setName('分类（逗号分隔）')

      .setDesc('文章分类，用英文逗号分隔')

      .addText(t => t.setValue(this.values.categories).onChange(v => this.values.categories = v));

  

    new Setting(contentEl)

      .setName('草稿')

      .setDesc('启用后文章不会立即公开')

      .addToggle(t => t.setValue(this.values.draft).onChange(v => this.values.draft = v));

  

    new Setting(contentEl)

      .setName('发布时间')

      .setDesc(`选择文章发布时间（当前偏移: ${this.publishOffset} 小时）`)

      .addText(t => t

        .setValue(toLocalDatetime(this.values.date))

        .onChange(v => {

          this.values.date = new Date(v);

        })

        .inputEl.setAttribute('type', 'datetime-local'));

  

new Setting(contentEl)

  .addButton(b => b.setButtonText(' 发布 ').setCta().onClick(() => {

    if (!this.values.title) { new Notice('标题不能为空'); return; }

    if (!this.values.categories || !this.values.categories.trim()) { // 修改点: 分类必填

      new Notice('分类不能为空，请至少填写一个分类');

      return;

    }

    this.close();

    const payload = {

      title: this.values.title,

      slug: this.values.slug, // slug 可为空，后面处理

      tags: this.values.tags.split(',').map(s => s.trim()).filter(Boolean),

      categories: this.values.categories.split(',').map(s => s.trim()).filter(Boolean),

      draft: !!this.values.draft,

      dateCreated: this.values.date,

    };

    this.onSubmit(payload);

  }));

  }

}

  
  
  

module.exports = class TypechoXmlRpcPublisher extends Plugin {

  async onload() {

    this.settings = Object.assign({

      endpoint: '',

      username: '',

      password: '',

      defaultBlogId: '0',

      useFrontmatter: true,

      useCurrentTime: true,

      publishTimeOffset: 0,

      syncTimeOffset: 0,

      managePostsCount: 20,

      frontmatterKeys: { title:'title', slug:'slug', tags:'tags', categories:'categories', draft:'draft', cid:'cid', dateCreated:'dateCreated' }

    }, await this.loadData() || {});

  

    this.addSettingTab(new (class extends require('obsidian').PluginSettingTab {

      constructor(app, plugin){ super(app, plugin); this.plugin = plugin; }

      display() {

        const { containerEl } = this;

        containerEl.empty();

        containerEl.createEl('h2', { text: 'Typecho XML-RPC 发布器设置' });

  

        const settingsInputs = {};

  

        settingsInputs.endpoint = new Setting(containerEl).setName('接口 URL')

          .setDesc('填写 Typecho XML-RPC 接口地址，例如：https://your-site.com/xmlrpc.php')

          .addText(t => t.setValue(this.plugin.settings.endpoint).onChange(v => settingsInputs.endpoint.value = v));

  

        settingsInputs.username = new Setting(containerEl).setName('用户名')

          .setDesc('用于登录 Typecho 的用户名，确保有发布权限')

          .addText(t => t.setValue(this.plugin.settings.username).onChange(v => settingsInputs.username.value = v));

  

        settingsInputs.password = new Setting(containerEl).setName('密码')

          .setDesc('对应用户名的密码，支持明文输入（将以安全方式保存）')

          .addText(t => t.setValue(this.plugin.settings.password).onChange(v => settingsInputs.password.value = v).inputEl.setAttribute('type','password'));

  

        settingsInputs.defaultBlogId = new Setting(containerEl).setName('默认博客ID')

          .setDesc('如果你的 Typecho 有多个博客，设置默认发布的博客 ID')

          .addText(t => t.setValue(this.plugin.settings.defaultBlogId).onChange(v => settingsInputs.defaultBlogId.value = v));

  

        settingsInputs.useCurrentTime = new Setting(containerEl).setName('总是使用当前时间作为发布时间')

          .setDesc('启用后发布文章时会忽略文章原有 Frontmatter 时间，直接使用当前时间')

          .addToggle(t => t.setValue(this.plugin.settings.useCurrentTime).onChange(v => settingsInputs.useCurrentTime.value = v));

  

        containerEl.createEl('h3', { text: '时间偏移设置' });

  

        settingsInputs.publishTimeOffset = new Setting(containerEl).setName('发布偏移（小时）')

          .setDesc('调整发布到 Typecho 的时间，例如 +8 表示服务器时间加 8 小时')

          .addText(t => t.setValue(String(this.plugin.settings.publishTimeOffset)).onChange(v => settingsInputs.publishTimeOffset.value = v));

  

        settingsInputs.syncTimeOffset = new Setting(containerEl).setName('同步偏移（小时）')

          .setDesc('同步 Typecho 发布时间时的时间偏移设置')

          .addText(t => t.setValue(String(this.plugin.settings.syncTimeOffset)).onChange(v => settingsInputs.syncTimeOffset.value = v));

  

        containerEl.createEl('h3', { text: '文章管理设置' });

  

        settingsInputs.managePostsCount = new Setting(containerEl).setName('文章管理显示数量') // 添加文章管理显示数量设置

          .setDesc('在文章管理界面显示的文章数量')

          .addText(t => t.setValue(String(this.plugin.settings.managePostsCount)).onChange(v => settingsInputs.managePostsCount.value = v));

  
  

        containerEl.createEl('h3', { text: 'Frontmatter 对应键' });

  

        const frontmatterKeys = {};

        ['title','slug','tags','categories','draft','cid','dateCreated'].forEach(k => {

          frontmatterKeys[k] = new Setting(containerEl).setName(k + ' 键')

            .setDesc(`对应 Frontmatter 中的 ${k} 键，用于自动读取或写入`)

            .addText(t => t.setValue(this.plugin.settings.frontmatterKeys[k]).onChange(v => frontmatterKeys[k].value = v));

        });

  

        new Setting(containerEl)

          .addButton(b => b.setButtonText('保存设置').setCta().onClick(async () => {

            this.plugin.settings.endpoint = settingsInputs.endpoint.value ?? this.plugin.settings.endpoint;

            this.plugin.settings.username = settingsInputs.username.value ?? this.plugin.settings.username;

            this.plugin.settings.password = settingsInputs.password.value ?? this.plugin.settings.password;

            this.plugin.settings.defaultBlogId = settingsInputs.defaultBlogId.value ?? this.plugin.settings.defaultBlogId;

            this.plugin.settings.useCurrentTime = settingsInputs.useCurrentTime.value ?? this.plugin.settings.useCurrentTime;

            this.plugin.settings.publishTimeOffset = parseFloat(settingsInputs.publishTimeOffset.value) || 0;

            this.plugin.settings.syncTimeOffset = parseFloat(settingsInputs.syncTimeOffset.value) || 0;

            this.plugin.settings.managePostsCount = parseInt(settingsInputs.managePostsCount.value) || 20;

  

            for (let k of ['title','slug','tags','categories','draft','cid','dateCreated']) {

              this.plugin.settings.frontmatterKeys[k] = frontmatterKeys[k].value ?? this.plugin.settings.frontmatterKeys[k];

            }

  

            await this.plugin.saveData(this.plugin.settings);

            new Notice('设置已保存');

          }));

      }

    })(this.app, this));

  

    this.addCommand({

      id: 'typecho-publish-current-note',

      name: '发布当前笔记到 Typecho',

      callback: () => this.publishActiveNote(),

    });

  

    this.addCommand({

      id: 'typecho-sync-publish-date',

      name: '同步 Typecho 发布时间',

      callback: () => this.syncPublishDate(),

    });

  

    this.addCommand({

      id: 'typecho-manage-posts',

      name: '管理文章',

      callback: () => new ManageModal(this.app, this).open(),

    });

  }

  

  async getActiveMDFile() {

    const file = this.app.workspace.getActiveFile();

    if (!file) throw new Error('未选中文件');

    if (file.extension !== 'md') throw new Error('当前文件不是 Markdown');

    return file;

  }

  

  async readFrontmatter(file) {

    const cache = this.app.metadataCache.getFileCache(file);

    return (cache && cache.frontmatter) || {};

  }

  

  async updateFrontmatter(file, updater) { await this.app.fileManager.processFrontMatter(file, updater); }

  

  extractContentWithoutFrontmatter(text) {

    if (text.startsWith('---')) {

      const end = text.indexOf('\n---', 3);

      if (end !== -1) return text.slice(end + 4).trimStart();

    }

    if (text.startsWith('+++')) {

      const end = text.indexOf('\n+++', 3);

      if (end !== -1) return text.slice(end + 4).trimStart();

    }

    return text;

  }

  

async publishActiveNote() {

  try {

    const file = await this.getActiveMDFile();

    const fm = await this.readFrontmatter(file);

    const raw = await this.app.vault.read(file);

    const body = this.extractContentWithoutFrontmatter(raw);

    const k = this.settings.frontmatterKeys;

  

    const preset = {

      title: fm[k.title] || file.basename,

      slug: fm[k.slug] || '', // slug 可为空，后面用 cid 填充

      tags: Array.isArray(fm[k.tags]) ? fm[k.tags] : (typeof fm[k.tags] === 'string' ? fm[k.tags].split(',').map(s=>s.trim()).filter(Boolean) : []),

      categories: Array.isArray(fm[k.categories]) ? fm[k.categories] : (typeof fm[k.categories] === 'string' ? fm[k.categories].split(',').map(s=>s.trim()).filter(Boolean) : []),

      draft: !!fm[k.draft],

      date: fm[k.dateCreated] ? XmlRpc.parseIso8601(fm[k.dateCreated]) : new Date(),

    };

  

    const proceed = async (vals) => {

      if (!vals.categories || vals.categories.length === 0) { // 修改点: 分类必填

        new Notice('分类不能为空，请至少填写一个分类');

        return;

      }

  

      const postStruct = {

        title: vals.title,

        description: body,

        mt_keywords: vals.tags.join(','),

        categories: vals.categories,

        post_type: 'post',

        wp_slug: vals.slug || '', // 先传空，等拿到 cid 再修正

        mt_allow_comments: 1,

      };

  

      let postDate = this.settings.useCurrentTime ? new Date() : (vals.dateCreated || new Date());

      if (this.settings.publishTimeOffset) {

        postDate = new Date(postDate.getTime() + this.settings.publishTimeOffset * 3600 * 1000);

      }

      postStruct.dateCreated = postDate;

  

      const endpoint = this.settings.endpoint.trim();

      const username = this.settings.username.trim();

      const password = this.settings.password;

  

      if (!endpoint) { new Notice('错误: 接口 URL 未设置'); console.error("未配置 endpoint"); return; }

      if (!username || !password) { new Notice('错误: 用户名或密码未设置'); console.error("账号配置缺失"); return; }

  

      const cidKey = k.cid;

      const existingCid = fm[cidKey];

      let method, params;

      if (existingCid) {

        method='metaWeblog.editPost';

        params=[String(existingCid), username, password, postStruct, !vals.draft];

      }

      else {

        method='metaWeblog.newPost';

        params=[String(this.settings.defaultBlogId || '0'), username, password, postStruct, !vals.draft];

      }

  

      try {

        const xml = XmlRpc.buildCall(method, params);

        const response = await requestUrl({ url:endpoint, method:'POST', headers:{'Content-Type':'text/xml'}, body:xml });

        const resultValue = XmlRpc.parseResponse(response.text);

  

        await this.updateFrontmatter(file, fm => {

          fm[k.title] = vals.title;

          fm[k.tags] = vals.tags;

          fm[k.categories] = vals.categories;

          fm[k.draft] = vals.draft;

          fm[k.dateCreated] = XmlRpc.iso8601(postStruct.dateCreated);

          fm['lastPublished'] = XmlRpc.iso8601(new Date());

          if (!existingCid) {

            fm[cidKey] = String(resultValue);

            // 修改点: 如果 slug 为空，用 cid 填充

            fm[k.slug] = vals.slug && vals.slug.trim() ? vals.slug : String(resultValue);

          } else {

            fm[k.slug] = vals.slug;

          }

        });

  

        new Notice(existingCid ? `更新文章成功 (cid=${existingCid})` : `发布新文章成功 (cid=${String(resultValue)})`);

      } catch (err) {

        console.error("发布失败: ", err, err.stack);

        new Notice(`发布失败: ${err.message || err}`);

      }

    };

  

    new PublishModal(this.app, preset, proceed, this.settings.publishTimeOffset, this.settings.useCurrentTime).open();

  } catch(e) {

    console.error("发布过程错误: ", e, e.stack);

    new Notice('错误: '+e.message);

  }

}

  

  async syncPublishDate() {

    try {

      const file = await this.getActiveMDFile();

      const fm = await this.readFrontmatter(file);

      const k = this.settings.frontmatterKeys;

      const cid = fm[k.cid];

      if (!cid) { new Notice('未找到文章 CID'); return; }

  

      const xml = XmlRpc.buildCall('metaWeblog.getPost', [ String(cid), this.settings.username, this.settings.password ]);

      const response = await requestUrl({ url:this.settings.endpoint, method:'POST', headers:{'Content-Type':'text/xml'}, body:xml });

      const result = XmlRpc.parseResponse(response.text);

      if (!result || !result.dateCreated) { new Notice('获取发布时间失败'); return; }

  

      let serverDate = XmlRpc.parseIso8601(result.dateCreated);

      if (this.settings.syncTimeOffset) serverDate = new Date(serverDate.getTime() + this.settings.syncTimeOffset*3600*1000);

      await this.updateFrontmatter(file, fm => { fm[k.dateCreated] = XmlRpc.iso8601(serverDate); });

  

      new Notice('发布时间已同步');

    } catch(e) { console.error(e); new Notice('错误: '+e.message); }

  }

};
```




## 三、使用Typecho插件保护XML-RPC接口

制作插件保护XML-RPC接口：https://www.hansjack.com/archives/typecho-xmlrpc-protector.html

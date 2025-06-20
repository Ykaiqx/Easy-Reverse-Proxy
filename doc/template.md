# 模板
## 工具
### generateProxyUrl
```TypeScript
function generateProxyUrl(url: string): string
```
1. 参数: `url`: 网址.
2. 说明: 生成原始网址的代理网址.
## 模板方法
### request
```typescript
interface TemplateRequest {
    header: object; // 请求头
    body: object; // 请求体,当Content-Type为x-www-form-urlencoded或json时存在
}
```
```TypeScript
request?: (option: TemplateRequest) => TemplateRequest
```
说明: 请求原始网址时调用.
### response
```typescript
interface TemplateResponse {
    header: object; // 响应头
    body: Buffer; // 响应体
}
```
```TypeScript
response?: (option: TemplateResponse) => Promise<TemplateResponse>
```
说明: 原始网址响应时调用,此方法会阻塞.
## 示例
```TypeScript
import type {Template} from '../util/model';
import {generateProxyUrl} from '../util/template';

let template: Template = {
    async response (option) {
        let body = option.body.toString();
        body = body.replaceAll('Product', 'Easy-Reverse-Proxy');
        let avatarUrl = 'https://avatars.githubusercontent.com/u/56395004?v=4';
        body = body.replaceAll(avatarUrl, generateProxyUrl(avatarUrl));
        option.body = Buffer.from(body);
        return option;
    }
};
export default template;
```
1. Product替换为Easy-Reverse-Proxy.
2. 生成 https://avatars.githubusercontent.com/u/56395004?v=4 的代理网址并替换.
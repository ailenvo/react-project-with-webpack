# Cài đặt Webpack và các loader
    yarn add webpack webpack-cli webpack-dev-server style-loader css-loader sass sass-loader file-loader typescript ts-loader -D
###### Giải thích
  -  ***webpack*** là phần lõi của webpack
  -  ***webpack-cli*** giúp ta gõ được lệnh của webpack trên terminal (gián tiếp thông qua file ***package.json***)
  -  ***webpack-dev-server*** hỗ trợ tạo một server localhost cho môi trường dev
  -  ***style-loader, css-loader*** giúp bạn có thể import được css vào file js
  -  ***sass, sass-loader*** giúp bạn biên dịch scss sang css
  -  ***file-loader*** giúp bạn import được các file ví dụ như ảnh, video vào file js
  -  ***typescript***: Phần lõi của ngôn ngữ Typescript
  -  ***ts-loader***: Giúp tích hợp Typescript vào webpack

# Cài đặt một số plugin bổ trợ webpack
    yarn add clean-webpack-plugin compression-webpack-plugin copy-webpack-plugin dotenv-webpack html-webpack-plugin mini-css-extract-plugin webpack-bundle-analyzer -D

###### Giải thích
   - ***clean-webpack-plugin***: Giúp dọn dẹp thư mục build trước khi build webpack. Ví dụ thư mục build của bạn đang chứa bản build trước, bây giờ bạn build lại thì plugin này sẽ xóa bản build trước.
   - ***compression-webpack-plugin***: Giúp nén các file css, js, html… thành ***gzip***
   - ***copy-webpack-plugin***: Giúp bạn copy các file ở thư mục dev vào thư mục build. Ví dụ bạn có các file như favicon.ico, robots.txt cùng cấp với index.html, bạn muốn khi build xong thì các file này cũng có mặt ở bản build. Nếu không có plugin này thì bạn phải copy thủ công.
   - ***dotenv-webpack***: Giúp bạn dùng được các biến môi trường ở file ***.env*** và trong app của bạn
   - ***html-webpack-plugin***: Giúp clone ra 1 file index.html từ file html ban đầu. Tại sao lại cần clone thì bạn có thể tham khảo bài Webpack
   - ***mini-css-extract-plugin***: Bình thường thì css sẽ nằm trong file js sau khi build. Và khi chạy app thì js sẽ thêm các đoạn css đó vào thẻ <code><style></style></code>. Muốn css phải nằm ở file riêng biệt với js và khi chạy app thì js sẽ tự import bằng thẻ <code>link</code>. Đó là chức năng của plugin này
   - ***webpack-bundle-analyzer***: Giúp phân tích bản build, coi thử thư viện nào đang chiếm bao nhiêu % bản build,…
# Cài đặt ESLint và Prettier
    yarn add eslint babel-eslint eslint-config-react-app eslint-loader eslint-plugin-flowtype eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react eslint-plugin-react-hooks @typescript-eslint/eslint-plugin @typescript-eslint/parser prettier eslint-plugin-prettier eslint-config-prettier -D
###### Giải thích
Ngoài prettier, eslint-config-prettier và eslint-plugin-prettier thì còn lại đều là các plugin tiêu chuẩn tương tự như bộ cài Create React App.

##### Thêm các file cấu hình
.eslintrc
        {
            "extends": ["react-app", "prettier"],
            "plugins": ["react", "prettier"],
            "rules": {
                "prettier/prettier": [
                "warn",
                {
                    "arrowParens": "avoid",
                    "semi": false,
                    "trailingComma": "none",
                    "endOfLine": "lf",
                    "tabWidth": 2,
                    "printWidth": 80,
                    "useTabs": false
                }
                ],
                "no-console": "warn"
            }
        }

.eslintignore
        /src/serviceWorker.ts
        /src/setupTests.ts

.prettierrc
        {
        "arrowParens": "avoid",
        "semi": false,
        "trailingComma": "none",
        "endOfLine": "lf",
        "tabWidth": 2,
        "printWidth": 80,
        "useTabs": false
        }

.prettierignore
        .cache
        package-lock.json

# Thêm script vào package.json
Chèn đoạn mã dưới đây vào mục scripts trong file ***package.json***
        "start": "webpack serve --mode development",
        "build": "webpack --mode production",
        "build:analyze": "webpack --mode production --env analyze",
        "lint": "eslint --ext js,jsx,ts,tsx src/",
        "lint:fix": "eslint --fix --ext js,jsx,ts,tsx src/",
        "prettier": "prettier --check \"src/**/(*.tsx|*.ts|*.jsx|*.js|*.scss|*.css)\"",
        "prettier:fix": "prettier --write \"src/**/(*.tsx|*.ts|*.jsx|*.js|*.scss|*.css)\"",

# Thêm file tsconfig.json để cấu hình Typescript
tsconfig.json
        {
            "compilerOptions": {
                "target": "ES5",
                "allowJs": true,
                "strict": true,
                "module": "ESNext",
                "moduleResolution": "node",
                "noImplicitAny": false,
                "sourceMap": true,
                "jsx": "react",
                "allowSyntheticDefaultImports": true,
                "baseUrl": ".",
                "paths": {
                "@/*": ["src/*"],
                "@@/*": ["./*"]
                }
            },
            "include": ["src/**/*"]
        }
# Thêm public/index.html
index.html
            <!DOCTYPE html>
            <html lang="en">
            <head>
                <meta charset="utf-8" />
                <link rel="icon" href="favicon.ico" />
                <meta name="viewport" content="width=device-width, initial-scale=1" />
                <meta name="theme-color" content="#000000" />
                <meta
                name="description"
                content="Web site created using create-react-app"
                />
                <link rel="apple-touch-icon" href="logo192.png" />
                <link rel="manifest" href="manifest.json" />
                <title>React App</title>
            </head>
            <body>
                <noscript>You need to enable JavaScript to run this app.</noscript>
                <div id="root"></div>
            </body>
            </html>

# Thêm file webpack.config.js
webpack.config.js

    const path = require("path")
    const webpack = require("webpack")
    const HtmlWebpackPlugin = require("html-webpack-plugin")
    const CopyPlugin = require("copy-webpack-plugin")
    const Dotenv = require("dotenv-webpack")
    const MiniCssExtractPlugin = require("mini-css-extract-plugin")
    const { CleanWebpackPlugin } = require("clean-webpack-plugin")
    const CompressionPlugin = require("compression-webpack-plugin")
    const BundleAnalyzerPlugin = require("webpack-bundle-analyzer")
    .BundleAnalyzerPlugin
    const fs = require("fs")
    const directoryPath = path.resolve("public")
    const handleDir = () => {
    return new Promise((resolve, reject) => {
        fs.readdir(directoryPath, (err, files) => {
        if (err) {
            reject("Unable to scan directory: " + err)
        }
        resolve(files)
        })
    })
    }
    module.exports = async (env, agrv) => {
    const isDev = agrv.mode === "development"
    const isAnalyze = env && env.analyze
    const dirs = await handleDir()
    const copyPluginPatterns = dirs
        .filter(dir => dir !== "index.html")
        .map(dir => {
        return {
            from: dir,
            to: "",
            context: path.resolve("public")
        }
        })
    const basePlugins = [
        new Dotenv(),
        new HtmlWebpackPlugin({
        template: "public/index.html"
        }),
        new CopyPlugin({
        patterns: copyPluginPatterns
        }),
        new MiniCssExtractPlugin({
        filename: isDev ? "[name].css" : "static/css/[name].[contenthash:6].css"
        }),
        new webpack.ProgressPlugin()
    ]
    let prodPlugins = [
        ...basePlugins,
        new CleanWebpackPlugin(),
        new CompressionPlugin({
        test: /\.(css|js|html|svg)$/
        })
    ]
    if (isAnalyze) {
        prodPlugins = [...prodPlugins, new BundleAnalyzerPlugin()]
    }
    return {
        entry: "./src/index.tsx",
        module: {
        rules: [
            {
            test: /\.(ts|tsx)$/,
            use: ["ts-loader", "eslint-loader"],
            exclude: /node_modules/
            },
            {
            test: /\.(s[ac]ss|css)$/,
            use: [
                MiniCssExtractPlugin.loader,
                {
                loader: "css-loader",
                options: { sourceMap: isDev ? true : false }
                },
                {
                loader: "sass-loader",
                options: { sourceMap: isDev ? true : false }
                }
            ]
            },
            {
            test: /\.(eot|ttf|woff|woff2)$/,
            use: [
                {
                loader: "file-loader",
                options: {
                    name: isDev ? "[path][name].[ext]" : "static/fonts/[name].[ext]"
                }
                }
            ]
            },
            {
            test: /\.(png|svg|jpg|gif)$/,
            use: [
                {
                loader: "file-loader",
                options: {
                    name: isDev
                    ? "[path][name].[ext]"
                    : "static/media/[name].[contenthash:6].[ext]"
                }
                }
            ]
            }
        ]
        },
        resolve: {
        extensions: [".tsx", ".ts", ".jsx", ".js"],
        alias: {
            "@": path.resolve("src"),
            "@@": path.resolve()
        }
        },
        output: {
        path: path.resolve("build"),
        publicPath: "/",
        filename: "static/js/main.[contenthash:6].js",
        environment: {
            arrowFunction: false,
            bigIntLiteral: false,
            const: false,
            destructuring: false,
            dynamicImport: false,
            forOf: false,
            module: false
        }
        },
        devtool: isDev ? "source-map" : false,
        devServer: {
        contentBase: "public",
        port: 3000,
        hot: true,
        watchContentBase: true,
        historyApiFallback: true,
        open: true
        },
        plugins: isDev ? basePlugins : prodPlugins,
        performance: {
        maxEntrypointSize: 800000 //  Khi có 1 file build vượt quá giới hạn này (tính bằng byte) thì sẽ bị warning trên terminal.
        }
    }
    }
###### Giải thích:
  -  ***isDev***: Chúng ta có 2 mode là ***development*** và ***production*** tương đương với ***dev*** và ***build***. 2 mode này được truyền vào thông qua <code>–mode</code> ở script trong ***package.json***.
  -  ***isAnalyze***: Nhìn vào file ***package.json*** chúng ta có câu lệnh ***build:analyze***, có truyền biến analyze vào webpack thông qua ***–env***. Biến này dùng để xác định bạn có dùng ***pluginBundleAnalyzerPlugin*** hay không.
  -  ***basePlugins***: Những plugins dùng trong mode development.
    Trong ***CopyPlugin*** ta thực hiện copy các file từ ***public*** sang thư mục ***build***
  -  ***CopyPlugin***: Copy mọi file trong thư mục ***public*** vào thư mục ***build***, ngoại trừ file ***index.html***. Vì ***index.html*** đã có ***HtmlWebpackPlugin*** thực hiện việc copy và generate code, nếu không loại trừ sẽ bị xung đột!.
  -  ***webpack.ProgressPlugin()*** giúp hiện % khi chạy webpack
    ***CompressionPlugin()*** giúp nén file build thành gzip, thỉnh thoảng bạn sẽ thấy một số file kích thước nhỏ không được nén, bạn có thể xem cấu hình nén và điều kiện được nén [tại đây](https://webpack.js.org/plugins/compression-webpack-plugin/)
  -  ***prodPlugins***: Những plugins dùng trong mode production.
  -  ***entry***: File đầu vào cho webpack, file này thường là file import mọi file khác
  -  ***module.rules***: Chứa các loader của webpack
    Chus ý chỗ <code>option.name</code> ở ***file-loader***: Đây là nơi có thể thay đổi tên và đường dẫn file sau khi build. Môi trường dev thì giữ nguyên tên và đường dẫn (như vậy khi inspect trên trình duyệt sẽ dễ dàng thấy nguồn gốc file từ đâu ra), còn môi trường production thì mình sẽ chuyển vào thư mục static.
  -  ***contenthash:6*** nghĩa là thêm 1 đoạn hash gồm 6 ký tự vào tên file.
  -  ***resolve.extensions***: Thứ tự ưu tiên các file khi import
  -  ***alias***: Tạo alias thuận tiện cho việc import trong webpack. Những nơi cần đường dẫn tuyệt đối thì phải dùng <code>path.resolve() hoặc path.join()</code>
  -  ***output.path***: Đường dẫn thư mục build.
  -  ***output.filename***: Tên file bundle sau khi được build. Cũng có thể quy định thư mục mà file build thuộc về
  -  ***output.publicPath***: Chứa đường dẫn tương đối mà từ file ***index.html*** trỏ đến các file trong thư mục build sau khi build. Lưu ý là file index.html được build nằm trong thư mục tên là build.
  -  ***output.environment***: Mặc định webpack generate ra code dùng 1 số cú pháp của ES6, nhưng target mong muốn là ES5 nên cần chỉnh một số thông số như sau.
  -  ***arrowFunction***: Hỗ trợ arrow function.
  -  ***bigIntLiteral***: Hỗ trợ BigInt
  -  ***const***: Hỗ trợ khai báo const và let
  -  ***destructuring***: Hỗ trợ destructuring
  -  ***dynamicImport***: Hỗ trợ async import
  -  ***forOf***: Hỗ trợ vòng lặp forOf cho các array
  -  ***module***: Hỗ trợ moudle ES6 (import … from ‘…’)’
  -  ***output.devtool***: tùy chọn sourcemap
  -  ***devServer.contentBase***: Chứa đường dẫn tương đối đến file index.html
  -  ***devServer.port***: port khi chạy localhost
  -  ***devServer.hot***: Chế độ hot reload. Mặc định thì ở dev server thì webpack sẽ refresh lại trang mỗi khi có thay đổi nhỏ trong code.
  -  ***devServer.publicPath***: Chứa đường dẫn tương đối từ thư mục root trỏ đến thư mục build (ở đây là dist). Chú ý phải thêm <code>/</code> ở trước và sau. Nhưng vì dùng ***HtmlWebpackPlugin*** nên ta sẽ tính từ chính thư mục dist. Vì thế giá trị cần dùng là <code>/</code>. Ở đây mình không dùng giá trị nào cả, vì mặc định nó đã là <code>/</code>
  -  ***devServer.watchContentBase***: Nếu bạn có thay đổi gì trong file index.html thì trình duyệt cũng tự động reload.
  -  ***devServer.historyApiFallback***: Phải set true nếu không khi bạn dùng lazyload module React thì sẽ gặp lỗi không load được file.
  -  ***plugins***: Chứa các plugin Webpack.
  -  ***performance.maxEntrypointSize***: Khi có 1 file build vượt quá giới hạn này (tính bằng byte) thì sẽ bị warning trên terminal.

Để chạy khi dev
        yarn start
Để build ra thành phẩm phục vụ deploy
        yarn build
Để build và phân tích source code
        yarn build:analyze
Ngoài ra bạn có thể yarn lint, yarn lint:fix, yarn prettier, yarn prettier:fix như đã định nghĩa trong file package.json


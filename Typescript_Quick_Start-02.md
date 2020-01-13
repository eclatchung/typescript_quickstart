# Typescript Quick Start

## 02 _ 개발 환경 구축

```bash
$ npm outdated -g typescript # 설치된 패키지가 최신 버전인지를 확인
```

```bash
$ npm uninstall -g typescript # 기존의 전역에 설치된 패키지 삭제
$ npm cache clean # 기존 설치에 사용됐던 패키지 설치 관련 파일을 삭제
$ npm install -g typescript # 최신 버전 다운로드
```



```types
let hello: string = "hello typescript!";
console.log(hello);
```

`tsc hello.ts & node hello.js`

--target 컴파일 옵션을 생략하면 ES3 버전을 기준으로 자바스크립트를 생성



### 2.2.2. tsc 명령어와 컴파일 옵션

```bash
$ tsc -p ../ # (--project) 지정한 경로를 프로젝트 루트 디렉터리로 설정하는 옵션
	# but tsconfig.json 파일이 없다면 단독으로 사용한 tsc 명령어가 제대로 동작하지 않는다
	# Cannot find a tsconfig.json file at the specified directory: '../'
	# tsconfig.json 파일명을 바꾸었다면 파일명을 직접 지정한다.
$ tsc -p ../tsconfig2.json

```



ECMA스크립트 버전별로 컴파일하기(-t 이용)

```bash
$ tsc hello.ts -t ES5 # ES5 버전으로 변환
	# ES3(default), ES5, ES6(ES2015), ES2016, ES2017 or ESNext
```



여러파일 컴파일

```bash
$ tsc file1.ts file2.ts

$ dir *.ts /b /s > ts-files.txt
$ tsc @ts-files.txt
```

```bash 
$ tsc file1.ts, file2.ts --out file3.js # 하나의 자바스크립트 파일로 합쳐서 생성
```



변경 감지 기능

```bash
$ tsc hello.ts -watch # (-w)
```



### 2.2.3. tsconfig.json 파일을 이용한 컴파일 

- tsconfig.json 파일이 위치한 경로가 타입스크립트 프로젝트의 루트 경로가 된다

```bash
$ tsc --init # 기본 tsconfig.json 파일이 만들어진다
```

```json
{
    "compilerOptions": {
        "target": "es6",  // 컴파일 후 변환될 ECMA스크립트 버전
        "module": "commonjs", // 모듈 형식을 지정한다
        "sourceMap": true, // 컴파일 후 맵(map) 파일의 생성 여부를 결정한다
        "strict": true, // true 이면 엄격 타입검사 모드를 활성화 한다
        "noImplicitAny": false, // any 타입으로 암묵적 형변환 여부를 결정한다. default: false
        "esModuleInterop": true, // ECMA스크립트 모듈과 상호 운용성을 가능하게 하는 속성. true: CommonJS 모듈을 디폴트 모듈처럼 호출할 수 있다.
        //"outFile": "./dist-specific/out.js", // files의 파일들의 컴파일 결과를 합쳐 생성
        "removeComments": false, // true: 주석은 모두 삭제된 채로 출력
        "outDir": "dist", // 컴파일 결과를 특정 디렉터리로 출력하도록 설정, outFile 속성과 대치되는 속성, ts 파일의 컴파일 결과를 1:1로 대응시켜 .js 파일로 저장.
        "pretty": true, //                 
        "importHelpers": true,
        "strictNullChecks": false,
        "noImplicitThis": true,
        "alwaysStrict": true,
        "noUnusedLocals": true,
        "noUnusedParameters": false,
        "noImplicitReturns": true,
        "noFallthroughCasesInSwitch": true,
        "moduleResolution": "node",
        "baseUrl": ".",
        "allowSyntheticDefaultImports": true,
        "experimentalDecorators": true,
        "emitDecoratorMetadata": true,
        "resolveJsonModule": true,
        "lib": [
            "es5",
            "es6",
            "dom",
            "es2015.core",
            "es2015.collection",
            "es2015.generator",
            "es2015.iterable",
            "es2015.promise",
            "es2015.proxy",
            "es2015.reflect",
            "es2015.symbol",
            "es2015.symbol.wellknown",
            "esnext.asynciterable"
        ],
        // "files":[ // 컴파일 할 파일 목록을 입력하면 명령어에 파일명을 열거하지 않아도 된다.
        //     "./src/hello1.ts",
        //     "./src/hello2.ts"
        // ],
        "include": [ // 컴파일 시 사용할 디렉터리를 설정, 생략시 모든 디렉터리 컴파일 대상
            "src/**/*",
            "src2/**/*"
        ],
        "exlude": [ // 컴파일 시 제외할 디렉터리를 지정
            "node_modules",
            ".vscode"
        ]
    }
}
```

- noImplicitAny: 타입을 지정하지 않은 변수는 자동으로 any 타입으로 추가. 따라서 타입을 선언하지 않아도 타입 에러가 발생하지 않는다.
- sourceMap: 맵 파일 생성여부. 맵 파일은 컴파일 과정에서 타입스크립트 파일과 자바스크립트 파일 간의 연결 정보를 담고있다. => 개발 과정에서 디버깅에 유용but 배포시 사용하지 않으므로 상황에 맞게 설정



#### tsconfig.json 속성 확장하기

config/base.json

```json
{
    "compilerOptions": {
        "removeComments": true,
        "sourceMap": true,
        "exlude": [ 
            "node_modules",
            ".vscode"
        ]
    }
}
```

tsconfig-extends.json

```json
{
    "extends": "./config/base.json",
    "compilerOptions": {
        "outFile": "./dist/extends/out.js",
        "target": "es5"
    }
}
```



### 2.2.4. ts-node 모듈을 이용한 컴파일과 실행

```bash
$ tsc hello.ts
$ node hello.js
	=>
$ ts-node hello.ts
```
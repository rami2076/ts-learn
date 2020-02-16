```
{
  "compilerOptions": {
    "target": "esnext",
    "module": "commonjs",
    "lib": ["esnext"],
    "types": ["node"],
    "strict": true,
    "importHelpers": true,
    "experimentalDecorators": true,
    "moduleResolution": "node",
    "esModuleInterop": true,
    "outDir": "dist"
  },
  "include": ["src"]
}
```

## 意味
compilerOptions :  
プロパティは省略可能であり、その場合はコンパイラのデフォルト値が使用されます。
[参考](http://js.studio-kingdom.com/typescript/project_configuration/compiler_options)
target:  
どの動作環境向けにトランスパイルするかを指定 
ESNextは、最新のES提案の機能がサポートされたバージョンを対象とする。  ES2019とする。
module:  
どのモジュールパターンで出力するか。ES2015
lib：
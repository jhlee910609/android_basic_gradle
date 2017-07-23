# [안드로이드 스튜디오] 그래들 (Gradle)

### 1. Compile 과정

- 리눅스 상에서의 Compile 및 Build 과정
- Linux Build = compile + Link (실행파일 생성)

### 2. Build

- 컴파일을 포함하는 개념으로 컴파일된 자원들을 패키징 하여 배포가능한 파일로 만듬
- 구글 플레이 스토어에 앱 올릴 때, 사인 되어야 함 (일종의 암호화 과정)
  - Debug 용인지 release용인지를 구분하여 keystore를 통해 암호화하는 과정을 거쳐야 함

### 3. Build Tool

- 자바에서는 아래 3가지의 빌드 도구가 가장 많이 사용됨
  - make, ant, mave, gradle
  - 각각 빌드 툴의 특징은 다음과 같음
  - 우리가 주로 사용하는 Gradle은 ant와 maven의 장점을 가져다 놓은 빌드 툴임

![](https://image.slidesharecdn.com/buildtools-introduction-141211014834-conversion-gate01/95/build-tools-introduction-5-638.jpg?cb=1418262589)

- 웹에서 Maven 자주 사용
  - 다루기가 쉬움
  - 문법만 보면 그래들보다 메이븐이 훨씬 쉬움
- Gradle 프로그래밍처럼 다룰 수 있음
  - 유연한 빌드 툴

### 4. 그래들

[그래들 참고 문서](https://goo.gl/UFVm61)

- Groovy DSL
  - Groovy 스타일 언어 지원 > 컴파일 없이 스크립트 실행
  - Domain Specific Language 기반 


- Gradle Wrapper
  - 실제 머신에 Gradle이 설치되어 있지 않아도 사용 가능
  - 별도로 그래들 설치하지 않아도 됨
  - gradlew 명령어로 그래들 바이너리를 자동으로 다운로드 하고 실행함
- Multi-Project
  - 멀티 프로젝트에 있는 서브 프로젝트 간의 의존관계를 정의할 수 있음

##### 4.1. 물리적 파일 - project

- build variants - relase 모드와 debug 모드가 나눠져 있음 
- relase > key / pw 정의 > 릴리스 

##### 4.2. .gitignore 파일에 작성되어 있는 파일들

- 업로드할 때, 무시할 애들 


##### 4.3. build.gradle

```java
// 모듈의 용도 구분
apply plugin: 'com.android.application'

// 앱 관련 설정
android {
    // 앱 버전 정의
    compileSdkVersion 25
    // 내가 사용하는 빌드 툴(그래들) 버전
    buildToolsVersion "25.0.2"
    // 앱 기본설정
    defaultConfig {
        // 대부분 패키지 명으로 사용
        applicationId "com.junhee.android.gradle"

        minSdkVersion 15
        // 최적화 시킨 OS 버전
        targetSdkVersion 25
        // 내 앱이 업그레이드 되었을 때, 숫자와 문자열 변경해주면 됨
        // 버전 코드 증가시키지 않으면 앱 자체가 마켓에 업로드 되지 않음
        versionCode 1
        // 0.9 beta, 1.0 사용화
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    // 개발과정에 따른 구분
    // 거의 release와 debug 주로 사용함
    signingConfigs {
        myConfig {
            storeFile file("/Users/JunHee/AndroidStudioProjects/keystore/keystore.jks")
            storePassword "wnsgml337!"
            keyAlias "testKey"
            keyPassword "wnsgml337!"
        }
    }

    buildTypes {
        debug {
            buildConfigField "String", "MYURL", "\"test.seoul.go.kr\""
        }
        release {
            signingConfig signingConfigs.release
            // type     // name  // real value (실제 값)
            buildConfigField "String", "MYURL", "\"seoul.go.kr\""
            minifyEnabled false
            // 난독화
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}
// 위의 항목 의외에 고려할 것들
// 앱 스토어(플레이 스토어, 앱스토어),
/*
productFlavors {
    skt() uplus() kt()
}
*/

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    androidTestCompile('com.android.support.test.espresso:espresso-core:2.2.2', {
        exclude group: 'com.android.support', module: 'support-annotations'
    })
    compile 'com.android.support:appcompat-v7:25.3.0'
    compile 'com.android.support.constraint:constraint-layout:1.0.2'
    testCompile 'junit:junit:4.12'
}
```

- [서명 보완 관련 참고 글](http://androidhuman.com/544)
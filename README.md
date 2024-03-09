
# whisper.cpp + llama.cpp 스위트 패키지

## 개요

[Package.swift](./Package.swift) 를 통한 라이브러리 제작
- whisper.cpp 와 llama.cpp 의 주요 기능을 라이브러리화 하는 패키지
- IOS 앱 제작시 패키지 사용하여 빌드



## 작업 히스토리

### 2024-03-07 

다음 커밋을 대상으로 취합 작업함: 2024-03-07

- [whisper.cpp: 25d313b3](https://github.com/ggerganov/whisper.cpp/commits/25d313b38b1f562200f915cd5952555613cd0110/)
- [llama.cpp: 8ced9f7e](https://github.com/ggerganov/llama.cpp/commit/8ced9f7e3225adb8501e9821ed1bbd92e3a5c7ae)

이유
- whisper.cpp 와 llama.cpp의 ggml 관련 구현의 시간차가 있기 때문임

작업 내용
- `whisper.cpp` 수정: llama.cpp 쪽 ggml 구현에 `enum_ggml_status` 도입에 따른 대응
    ```cpp
    static bool ggml_graph_compute_helper(
        struct ggml_backend * backend,
            struct ggml_cgraph * graph,
                        int   n_threads) {
        if (ggml_backend_is_cpu(backend)) {
            ggml_backend_cpu_set_n_threads(backend, n_threads);
        }
    #ifdef GGML_USE_METAL
        if (ggml_backend_is_metal(backend)) {
            ggml_backend_metal_set_n_cb(backend, n_threads);
        }
    #endif
        // @@--- Modification by Jangmin Oh:
        // llama.cpp 와 통합하기 위한 조치
        return ggml_backend_graph_compute(backend, graph) == GGML_STATUS_SUCCESS;
        // ---@@
    }
    ```
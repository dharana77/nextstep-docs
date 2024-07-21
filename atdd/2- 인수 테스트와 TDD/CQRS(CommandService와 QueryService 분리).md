# LineCommandService와 LineQueryService 분리 이유 (CQRS)

## 개요
이 문서에서는 `LineCommandService`와 `LineQueryService`를 분리하는 이유와 그 장점에 대해 설명합니다. 이러한 분리는 명령-질의 책임 분리(CQRS, Command Query Responsibility Segregation) 패턴에 기반합니다.

## LineCommandService

`LineCommandService` 인터페이스는 라인 데이터를 **관리**하는 데 중점을 둡니다. 이 서비스는 라인의 생성, 업데이트, 삭제 및 라인 내 섹션 관리를 위한 메서드를 포함합니다.

### 주요 메서드
- `saveLine(LineRequest lineRequest)`: 새로운 라인을 생성합니다.
- `updateLine(Long id, LineRequest lineRequest)`: 기존 라인을 업데이트합니다.
- `deleteLineById(Long id)`: 라인을 삭제합니다.
- `addSection(Long lineId, SectionRequest sectionRequest)`: 라인에 새로운 섹션을 추가합니다.
- `removeSection(Long lineId, Long stationId)`: 라인에서 섹션을 제거합니다.

## LineQueryService

`LineQueryService` 인터페이스는 라인 데이터를 **조회**하는 데 중점을 둡니다. 이 서비스는 특정 기준에 따라 라인 정보를 가져오는 메서드를 포함하고 있습니다.

### 주요 메서드
- `findLineById(Long id)`: ID로 라인을 조회합니다.
- `findAllLines()`: 모든 라인을 조회합니다.

## 분리의 이유

### 장점

1. **성능 최적화**
    - **읽기와 쓰기 작업의 분리**: 각 작업에 최적화된 방식으로 처리할 수 있습니다. 예를 들어, 쓰기 데이터베이스는 3차 정규형으로 최적화하고, 읽기 데이터베이스는 비정규화하여 빠르게 조회할 수 있습니다.

2. **확장성**
    - **독립적인 확장**: 읽기와 쓰기 서비스 또는 데이터베이스를 독립적으로 확장할 수 있어, 특정 부분에 대한 부하를 효율적으로 관리할 수 있습니다.

3. **단일 책임 원칙 (Single Responsibility Principle)**
    - **책임의 분리**: 각 서비스가 명령(쓰기)과 질의(읽기) 작업을 별도로 처리함으로써, 코드의 복잡성을 줄이고 유지보수성을 높일 수 있습니다.

4. **보안 관리 용이**
    - **보안 및 권한 관리**: 읽기와 쓰기 작업을 분리함으로써, 각 작업에 대한 보안 정책을 별도로 적용할 수 있습니다.

### 단점

1. **복잡성 증가**
    - **시스템 복잡성**: 읽기와 쓰기 작업을 분리함으로써 시스템의 복잡성이 증가할 수 있습니다.

2. **데이터 일관성 문제**
    - **최종 일관성**: 쓰기와 읽기 데이터베이스가 분리되어 있을 경우, 데이터가 즉시 일관성을 가지지 않을 수 있습니다.

3. **적용의 한계**
    - **간단한 프로젝트에는 부적합**: CQRS 패턴은 복잡한 시스템에 적합하며, 단순한 CRUD 기반 애플리케이션에는 오히려 불필요한 복잡성을 초래할 수 있습니다.

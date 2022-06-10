# Korean-translation-of-Edgar-Cod-1970-

 이 레포지토리는 IBM 수학자 에드거 커드의 1970년 논문 A relational data model for large shared data banks을 한국어로 번역한 것입니다.
 
이 논문은 관계형 데이터베이스의 이론적 토대를 제공하였으며, 현대의 데이터 베이스 중 관계형 데이터베이스가 차지하는 비중이 높은 만큼 이 논문의 가치 역시 대단하다고 생각합니다.

하지만 해당 논문의 한국어 버전을 찾을 수 없었고, 동시에 수학을 전공한 저는 기본적인 수학 지식으로도 충분히 논문을 읽을 수 있도록 번역하고자 결심했습니다.

저는 이 레포지토리를 통해 다양한 한국 개발자들이 관계 데이터베이스의 근본이 되는 에드거 커드의 논문을 쉽게 읽을 수 있으면 합니다.

이 레포지토리는 개발을 배운지 얼마 안된 초보 개발자의 글이며, 기술적인 오류가 존재할 수 있습니다. 의견 보내주신다면 지속적으로 수정하도록 하겠습니다.

또한 초기 내용은 번역기를 사용하여 문맥 상 문제가 있을 수 있습니다. 이 역시 지속적으로 수정하고, 다듬고자 합니다.

- 2022. 06. 10. / 1.3 중간까지 번역

## 목차   
### 0. 개요   
### 1. 관계형 모델과 일반적 형태

 - 1.1. 도입   
 - 1.2. 현재 시스템들의 데이터 의존성   
 
     - 1.2.1. 순서 종속성   
     - 1.2.2. 인덱싱 종속성   
     - 1.2.3. 엑세스 경로 종속성   
 - 1.3. 데이터의 관계적 관점   

 - 1.4. 일반적 형태   
 
 - 1.5. 몇가지 언어적 측면   
 
 - 1.6. 표현가능, 명명된, 그리고 저장된 관계   
### 2. 중복성과 일관성   




# 거대 데이터 뱅크를 위한 관계 데이터 모델 - A Relational Model of Data for Large Shared Data Banks

## 0. 개요
 향후 거대한 데이터 뱅크 사용자들은 장비 속의 데이터가 어떻게 조직 되는지(내부표현)를 알아야 할 필요가 없어야 합니다. 그런 정보를 제공하도록  유도하는 서비스는 바람직한 서비스가 아닙니다. 터미널과 대부분의 응용 프로그램 프로그램에서 사용자의 활동은 데이터 내부 표현이 변경되거나 심지어 외부표현 일부가 변경되어도 영향을 받지 않아야 합니다. 쿼리나 업데이트의 변화, 트레픽 리포트, 저장된 정보가 자연스럽게 커진 경우로 인해 데이터 표현은  자주 변할 수 밖에 없습니다.
 
기존의 비추론적이고, 포맷된 데이터 시스템은 유저에게 트리 구조의 파일이나 좀 더 일반적인 네트워크 모델을 제공했습니다. 첫 번째 섹션에서 이런 모델들의 부적절성을 논의하고자 합니다. n항 관계 모델과 일반적인 데이터 베이스 관계의 형태, 그리고 하위언어를 통한 범용 데이터 컨셉에 대해 소개하고자 합니다. 두 번째 섹션에서는 관계(논리적 인터페이스 이외의)의 특정 연산을 논의하고, 유저 모델에서의 중복성과 일관성 문제를 다루고자 합니다.

 Future users of lage data banks must be protected from having to know how the data is organized in the machine(the internal representation). A prompting service which supplies such information is not a satisfactory solution. Activities of users at terminals and most application programs should remain unaffected when the internal representation of data is changed and even when some aspects of the external representation are changed. Changes in data representation will often be needed as a result of changes in query, update, and report traffic and natural growth in the types of stored information.
 Existing noniferential, formatted data systems provide users with tree-structured files or slightly more general network models of the data. In Section1, inadequacies of these models are discussed. A model based on n-ary relations, a normal form for dat base relations, and the concept of a universal data sublanguage are introduced. In Section 2, certain operations on relations(other than logical inference) are discussed and applied to the problems of redundancy and consistency in the user’s model.

KEY WORDS AND PHRASES : data bank, data base, data structure, data organization, hierarchies of data, networks of data, relations, derivability, redundancy, consistency, composition, join, retrieval language, predicate calculus, security, data integrity

## 1. 관계형 모델과 일반적 형태 - Relational Model and Normal Form
### 1.1 도입 - INTRODUCTION
이 논문은 거대한 데이터 포멧 뱅크의 접근을 나누어 제공할 수 있는 시스템에 대한 기본적인 관계 이론을 적용에 대한 내용입니다. Childs [1]의 논문을 제외하고, 데이터 시스템에 관계를 적용시키는 주된 내용은 연역적인 질문 - 응답 시스템에 있었습니다. Levein and Maron [2]는 이 분야 작업에 필요한 수많은 자료를 제공해주었습니다. 이와 대조적으로, 여기서 다뤄지는 문제는 데이터 독립성(데이터 유형의 성장과 데이터 표현의 변화로부터 응용프로그램의 독립성과 터미널 활동의 독립성)와 nondeductive systems(비연역적 시스템)에서 문제가 될 것으로 보이는 특정 종류의 데이터 불일치입니다.
 섹션1의 데이터에 대한 관계적 관점(모델)은 현제 비추론시스템에서 유행중인 그래프나 네트워크 모델[3, 4]에 비해 여러가지 측면에서 더 뛰어날 것으로 보입니다. 관계 모델의 자연적 구조(즉, 기계 표현 포즈를 위한 추가적인 구조를 곂치지 않는 것)만을 가지고 데이터를 묘사할 수 있도록 제공합니다. 따라서, 그것은 프로그램과 다른 한편으로는 기계 표현 그리고 데이터 구성 사이의 독립성을 최대한 산출할 수 있는 높은 수준의 데이터 언어의 기초를 제공합니다.
 
 관계적 관점의 또 다른 장점은 파생 가능성, 중복성 및 관계의 일관성을 다루기 위한 건전한 기초를 형성한다는 것입니다.(이것들은 섹션 2에서 논의된다). 한편, 네트워크 모델은 수 많은 혼란을 가져왔는데, 그 중에서도 특히 연결의 파생을 관계의 파생으로 착각하고 있다는 것이다.("연결 트랩" 섹션 2의 비고 참조).
 마지막으로, 관계적 관점은 현재 형식화된 데이터 시스템의 범위와 논리적 한계 그리고 단일 시스템 내에서 경쟁적인 데이터 표현의 상대적 장점(논리적 관점에서)을 더 명확하게 평가할 수 있게 한다. 이보다 더 명확한 관점의 사례는 이 논문의 여러 부분에서 인용됩니다. 관계형 모델을 지원하는 시스템의 구현은 논의되지 않습니다.

 This paper is concerned with the application of elementary relation theory to systems which provide shared access to large banks of formatted data. Except for a paper by Childs [1], the principal application of relations to data system has been to deductive question-answering systems. Levein and Maron [2] provide numerous references to work in this area.
 In contrast, the problems treated here are those of data independence - the independence of application programs and terminal activities from growth in data types and changes in data representation- and certain kinds of data inconsistency which are expected to become troublesome even in nondeductive systems.
 The relational view (or model) of data described in Section 1 appears to be superior in several respects to the graph or network model [3, 4] presently in vogue for noninferential systems. It provides a means of describing data with its natural structure only -- that is, without superimposing any additional structure for machine representation poses. Accordingly, it provides a basis for a high level data language which will yield maximal independence between programs on the one hand and machine representation and organization of data on the other.
 A further advantage of the relational view is that it forms a sound basis for treating derivability, redundancy, and consistency of relations - these are discussed in Section 2. The network model, on the other hand, has spawned a number of confusions, not the least of which is mistaking the derivation of connections for the derivation of relations (see remarks in Section 2 on the "connection trap" ).
 Finally, the relational view permits a clearer evaluation of the scope and logical limitations of present formatted data systems, and also the relative merits (from a logical standpoint) of competing representations of data within a single system. Examples of this clearer perspective are cited in various parts of this paper. Implementations of systems to support the relational model are not discussed.
 
 ### 1.2 현재 시스템에서의 데이터 의존성 - Data Dependence in Present Systems
  최근 개발된 정보 시스템에서 데이터 설명 표의 제공은 데이터 독립성이라는 목표를 위한 주된 발전을 보여 줍니다.[5, 6, 7]. 이러한 테이블들은 데이터 뱅크에 저장된 데이터 표현의 일부 특성들의 변경을 용이하게 합니다. 그러나, 응용 프로그램을 논리적으로 손상시키지 않고 변경할 수 있는 데이터 표현 특성의 다양성은 여전히 상당히 제한적입니다. 게다가, 사용자 상호작용 데이터 모델은 특히 데이터 콜렉션들의(개별 항목이 아닌) 표현에 관한 표현적인 특징으로 여전히 어수선합니다. 여전히 제거해야 하는 데이터 종속성의 세 가지 주요 유형은 순서 종속성, 인덱싱 종속성 및 액세스 경로 종속성입니다. 일부 시스템에서 이 종속성들은 서로 명확하게 분리할 수 없습니다.
  
   The provision of data description tables in recently developed information systems represents a major advance toward the goal of data independence [5, 6, 7]. Such tables facilitate changing certain characteristics of the data representation stored in a data bank. However, the variety of data representation characteristics which can be changed without logically impairing some application programs is still quite limited. Further, the model of data with which users interact is still cluttered with representational properties, particularly in regard to the representation of collections of data (as opposed to individual items ). Three of the principal kinds of data dependencies which still need to be removed are: ordering dependence, indexing dependence, and access path dependence. In some systems these dependencies are not clearly separable from one another.
   
#### 1.2.1 순서 종속성 - Ordering Dependence
 데이터 뱅크의 데이터 요소는 다양한 방식으로 저장될 수 있으며, 일부는 필연적으로 순서에 대한 고려(concern)가 없으며, 일부는 각 요소가 하나의 순서에만 참여하도록 허용하고, 다른 일부는 각 요소가 여러 순서에 참여하도록 허용합니다. 데이터 요소가 유선 연결된 주소의 순서와 밀접하게 연관된 적어도 하나 이상의 전체 순서로 저장을 요구하거나 저장될 수 있도록 허용하는 기존 시스템을 고려해 봅시다. 예를 들어, 부품 관련 파일의 레코드는 부품의 일련 번호에 따라 오름차순으로 저장할 수 있습니다. 이러한 시스템은 일반적으로 응용 프로그램들이 저장 순서와 똑같은 파일로부터 기록이 제출된 순서를 추측할 수 있을 것입니다. 파일의 저장 순서를 활용하는 응용 프로그램들은 어떤 이유로 인해 다른 순서로 대체해야 한다면 제대로 작동하지 않을 수 있습니다. 포인터 방식으로 구현된 저장 순서 역시  이에 대해 유사한 언급이 존재합니다.
 
 오늘날 유통되는 모든 잘 알려진 정보 시스템이 제출 순서와 저장 순서를 명확하게 구분하지 못하기 때문에 어떤 특정 시스템을 예로 들 필요가 없습니다. 이러한 종류의 독립성을 제공하기 위해 중요한 구현 문제가 해결되어야 합니다.
 
  Elements of data in a data bank may be stored in a variety of ways, some involving no concern for ordering, some permitting each element to participate in one ordering only, others permitting each element to participate in several orderings. Let us consider those existing systems which either require or permit data elements to be stored in at least one total ordering which is closely associated with the hardwired-determined ordering of addresses. For example, the records of a file concerning parts might be stored in ascending order by part serial number. Such systems normally permit application pro- grams to assume that the order of presentation of records from such a file is identical to (or is a subordering of) the stored ordering. Those application programs which take advantage of the stored ordering of a file are likely to fail to operate correctly if for some reason it becomes necessary to replace that ordering by a different one. Similar remarks hold for a stored ordering implemented by means of pointers.
  
 It is unnecessary to single out any system as an example, because all the well-known information systems that are marketed today fail to make a clear distinction between order of presentation on the one hand and stored ordering on the other. Significant implementation problems must be solved to provide this kind of independence.
 
 ### 1.2.2 인덱싱 종속성 - Indexing Dependence
  포맷된 데이터의 맥락에서 인덱스는 보통 순수하게 성능 지향적인 데이터 표현 구성 요소로 봅니다. 이는 쿼리 및 업데이트에 대한 응답을 개선하고 동시에 삽입이나 삭제에 대한 응답을 느리게 하는 경향이 있습니다. 정보 관점에서 인덱스는 데이터 표현에서 불필요한 구성요소입니다. 만약 시스템이 모든 인덱스를 사용하고 데이터 뱅크 속 활동 패턴이 변화하는 환경에서 잘 작동하려면, 인덱스를 수시로 만들고 파괴할 수 있는 기능이 필요할 것입니다. 그럼 다음 의문이 듭니다. 응용 프로그램 및 터미널 활동들이 인덱스가 오고 가는 동안 변함없이 유지될 수 있을까?
  
 현재 포맷된 데이터 시스템은 인덱싱에 대해 매우 넓은 접근 방식을 취합니다. TDMS[7]는 모든 속성의 인덱싱을 반드시 제공합니다. 현재 출시된 IMS[5] 버전은 사용자에게 인덱싱이 아에 없는 파일(계층적 순차적인 구성) 또는 기본직인 키만 인덱싱한 파일(계층적 인덱스된 순차적인 구성)에 대한 선택권을 제공합니다. 어떤 경우에도 사용자의 응용프로그램의 로직은 무조건적으로 제공되는 인덱스의 존재에 의존하지 않는다. 그러나 IDS[8]는 파일 설계자가 인덱스 속성을 선택하고 추가적인 채인(연결)을 통해 파일 구조에 인덱스를 통합할 수 있도록 허용한다. 이러한 인덱싱 체인의 성능 이점을 활용하는 응용 프로그램에서는 이름을 통해 체인을 참조해야 합니다. 이러한 체인을 나중에 제거하면 이러한 프로그램이 제대로 작동하지 않습니다.
 
  In the context of formatted data, an index is usually thought of as a purely performance-oriented component of the data representation. It tends to improve response to queries and updates and, at the same time, slow down response to insertions and deletions. From an informational standpoint, an index is a redundant component of the data representation. If a system uses indices at all and if it is to perform well in an environment with changing patterns of activity on the data bank, an ability to create and destroy indices from time to time will probably be necessary. The question then arises: Can application programs and terminal activities remain invariant as indices come and go?
  
 Present formatted data systems take widely different approaches to indexing. TDMS[7] unconditionally provides indexing on all attributes. The presently released version of IMS[5] provides the user with a choice for each file: a choice between no indexing at all (the hierarchic sequential organization) or indexing on the primary key only (the hierarchic indexed sequential organization). In neither case is the user's application logic dependent on the existence of the unconditionally provided indices. IDS[8], however, permits the file designers to select attributes to be indexed and to incorporate indices into the file structure by means of additional chains. Application programs taking advantage of the performance benefit of these indexing chains must refer to those chains by name. Such programs do not operate correctly if these chains are later removed.
 
 ### 1.2.3. 엑세스 경로 종속성 - Access Path Dependence
  기존의 많은 포맷된 데이터 시스템은 사용자에게 트리 구조의 파일이나 데이터의 약간 더 일반적인 네트워크 모델을 제공한합니다. 이러한 시스템과 함께 작동하도록 개발된 응용 프로그램들은 트리나 네트워크 구조가 변경되면 논리적으로 손상되는 경향이 있습니다. 다음은 간단한 예가 있습니다.
  
 데이터 뱅크에 부품 과 프로젝트에 대한 정보가 포함되어 있다고 가정합니다. 각 부품에 대해 부품 번호, 부품 이름, 부품 설명, 재고 수량과 주문 수량이 기록됩니다. 각 프로젝트에 대해서는 프로젝트 번호, 프로젝트 이름, 프로젝트 설명이 기록됩니다. 프로젝트가 특정 부품을 사용할 때마다 해당 프로젝트에 할당된 부품의 수량도 기록됩니다. 시스템이 사용자나 파일 설계자에게 트리 구조의 관점에서 데이터를 선언하거나 정의해야 한다고 가정해봅시다. 그런뒤, 위에서 언급한 정보를 위해 계층 구조 중 하나를 채택할 수 있습니다(구조물 1-5 참조).
 
 이제 이름이 "alpha"인 프로젝트에서 사용되는 각각의 모든 부품에 대해 부품 번호, 부품 이름, 할당된 수량을 출력하는 문제를 생각해 보십시오. 이 문제를 해결하기 위해 사용 가능한 트리 지향 정보 시스템 중 무엇을 선택하는지에 관계없이 다음과 같이 생각해 볼 수 있습니다. 만약 프로그램 P가 이 문제를 위해 위의 다섯 가지 구조 중 하나를 가정하여 개발된다면, P는 어떤 구조가 유효한지를 결정하기 위한 테스트를 하지 않는다 - 그러면 P는 나머지 구조 중 적어도 세 개에서 실패할 것이다. 더 구체적으로, 만약 P가 구조 5에서 성공한다면, P는 다른 경우 모두 실패할 것이다; 만약 P가 구조 3 또는 4에서 성공한다면, 그것은 적어도 1, 2, 5에서 실패할 것이다; 만약 P가 1에서 성공한다면, 그것은 적어도 3, 4, 5에서 실패할 것이다. 그 이유는 각각의 경우에 간단하다. 어떤 구조가 유효한지 확인하는 테스트가 없는 경우, 존재하지 않는 파일에 대한 참조를 실행하려고 시도하거나(사용 가능한 시스템에서 이를 오류로 처리함) 필요한 정보를 포함하는 파일에 대한 참조를 실행하려고 시도하지 않기 때문에 P는 실패합니다. 확신이 서지 않는 독자는 이 간단한 문제에 대한 샘플 프로그램을 개발해보시기 바랍니다.
 
 일반적으로 시스템에 의해 허용된 모든 트리 구조를 테스트하는 응용 프로그램을 개발하는 것은 실용적이지 않기 때문에 이러한 프로그램은 구조의 변경이 필요할 때 실패합니다.
 
 사용자에게 데이터의 네트워크 모델을 제공하는 시스템도 유사한 문제에 부딪힙니다. 트리와 네트워크 사례 모두에서 사용자(또는 그의 프로그램)는 데이터에 대한 사용자 액세스 경로 모음을 이용해야 합니다. 이러한 경로가 IDS에서 관련성은 매우 단순하지만 TDMS에서는 정반대인 포인터(저장된 표현에서 정의된 경로)와 밀접하게 일치하는지 여부는 중요하지 않습니다.  그 결과는 저장된 표현과 관계없이 터미널 활동과 프로그램이 사용자 접근 경로의 지속적 존재에 따라 달라집니다. 
 
 이에 대한 한 가지 해결책은 일단 사용자 접근 경로가 정의되면 그 경로를 사용하는 모든 응용 프로그램들이 쓸모없게 될 때까지 그것은 쓸모없게 되지 않을 것이라는 정책을 채택하는 것입니다. 이러한 정책은 실용적이지 않습니다. 이는 데이터 뱅크의 사용자 커뮤니티를 위한 전체 모델에서 접근 경로의 수가 결국 과도하게 커질 것이기 때문입니다.

 Many of the existing formatted data systems provide users with tree-structured files or slightly more general network models of the data. Application programs developed to work with these systems tend to be logically impaired if the trees or networks are changed in structure. A simple example follows.
 Suppose the data bank contains information about parts and projects. For each part, the part number, part name, part description, quantity-on-hand, and quantity-on-order are recorded. For each project, the project number, project name, project description are recorded. Whenever a project makes use of a certain part, the quantity of that part committed to the given project is also recorded. Suppose that the system requires the user or file designer to declare or define the data in terms of tree structures. Then, any one of the hierarchical structures may be adopted for the information mentioned above (see Structures 1- 5).
 
Now, consider the problem of printing out the part number, part name, and quantity committed for every part used in the project whose project name is "alpha." The following observations may be made regardless of which available tree-oriented information system is selected to tackle this problem. If a program P is developed for this problem assuming one of the five structures aboveÑthat is, P makes no test to determine which structure is in effect - then P will fail on at least three of the remaining structures. More specifically, if P succeeds with structure 5, it will fail with all the others; if P succeeds with structure 3 or 4, it will fail with at least 1, 2, and 5; if P succeeds with 1 or 2, it will fail with at least 3, 4, and 5. The reason is simple in each case. In the absence of a test to determine which structure is in effect, P fails because an attempt is made to execute a reference to a nonexistent file (available systems treat this as an error) or no attempt is made to execute a reference to a file containing needed information. The reader who is not convinced should develop sample programs for this simple problem.

 Since, in general, it is not practical to develop application programs which test for all tree structuring permitted by the system, these programs fail when a change in structure becomes necessary.
 
 Systems which provide users with a network model of the data run into similar difficulties. In both the tree and network cases, the user (or his program) is required to exploit a collection of user access paths to the data. It does not matter whether these paths are in close correspondence with pointer - (defined paths in the stored representation - in IDS the correspondence is extremely simple, in TDMS it is just the opposite. The consequence, regardless of the stored representation, is that terminal activities and programs become dependent on the continued existence of the user access paths.
 One solution to this is to adopt the policy that once a user access path is defined it will not be made obsolete until all application programs using that path have become obsolete. Such a policy is not practical, because the number of access paths in the total model for the community of users of a data bank would eventually become excessively large.
 
 ### 1.3 데이터의 관계적 관점
  여기서 관계라는 용어는 수학적인 의미로 채택된 것을 사용한다. 집합 S1, S2, … , Sn이 주어졌을 때 (반드시 구분된 것은 아니다.) , S1에서 첫번째 원소를 갖고, S2에서 두번째 원소를 갖고, Sn에서 n번째 원소를 갖는 n개의 집합 R은 n개의 집합의 관계이다.( 좀 더 간결하게 말하자면, R은 n개의 집합의 카테시안 곱 S1 X S2 X … X Sn의 부분집합이다.) 우리는 Sj를 R의 j번째 도메인이라 할 수 있을 것이다. 위의 정의에 따라, R은 n차원을 갖는다고 할 수 있다. 1차원의 관계는 1항, 2차원은 2항, 3차원은 3항, n차원은 n항이라고 불린다.
  
 설명을 위해, 우리는 자주 관계를 배열적인 표현으로 사용할 것이지만, 이는 관계적 관점의 필수적인 부분이 아니라는 것을 기억해야 한다. N 항 관계 R을 나타내는 배열은 다음과 같은 성질을 가진다.
 
1. 각 행은 R의 n개 쌍을 표현합니다.   
2. 행의 순서는 중요하지 않습니다.   
3. 모든 행은 다릅니다.   
4. 열의 순서가 중요합니다. 이는 S1, S2, … , Sn에 해당합니다.(그러나 순서가 있는 도메인과 순서가 없는 도메인의 관계를 아래에서 보십시오.)   
5. 각 열의 중요성은 해당 도멘인의 이름으로 레이블을 지정함으로써 부분적으로 전달됩니다.   

그림1의 예는 구체적으로 명시된 양을 특정한 공급업체에서 특정 프로젝트로의 특정 부품을 공급하는 관계 차원을  보여줍니다.

공급	공급업체	부품	프로젝트	수량
	1	2	5	17
	1	3	5	23
	2	3	7	9
	2	7	5	4
	4	1	1	12
- 그림 1 4차원 관계

 다음과 같은 질문을 할 수 있습니다. 만약 열이 일치한 도메인의 이름으로 레이블 지정된다면, 열의 순서가 왜 중요할까? 그림 2를 예로 보자면, 두개의 열이 동일한 제목(동일한 도메인)을 가질 순 있지만, 관계에서는 분명한 의미를 갖는다. 묘사된 관계는 구성요소(component)라고 불린다. 그것은 3항 관계이며, 첫째 두 도메인은 부품이고 세 번째 도메인은 수량이라 한다. 구성요소(x, y, z)	의 의미는 부품 x가 부품 y의  가까운 구성요소(하위품목)이고, 부품 x의 z 유닛은 부품 y하나가 모여야할 필요가 있다는 것이다. 이것은 부품 폭발 문제에서 중요한 역할을 하는 관계입니다.
 
component	부품	부품	수량
	1	5	9
	2	5	7
	3	5	2
	2	6	12
	3	6	3
	4	7	1
	6	7	1
- 그림2. 2가지 동일한 도메인들

 여러 존재하는 정보 시스템(주로 트리 구조의 파일 기반)이 두개 이상의 같은 이름을 갖는 도메인과의 관계를 표현하는 것을 제공하는데 실패하는 것은 주목할 만한 사실이다. IMS/360 [5]의 현재 버전이 이런 시스템의 예시입니다.
 
 데이터 뱅크에서 데이터의 총량은 여러 시간의 관계의 수집을 통해 볼 수 있다. 이 관계는 여러 차원입니다. 시간이 지남에 따라, 각 n항 관계는 추가적인 n 튜플이 삽입되거나 삭제될 수 있고, 기존의 n 튜플의 하나 또는 모든 구성요소가 변화할 수 있습니다.
 
  그러나 많은 상업, 정부기관, 과학 데이터 뱅크의 일부 관계들은 꽤나 높은 차원입니다.(30차원 정도는 드문 일이 아닙니다.) 사용자는 일반적으로 어떤 관계의 도메인 순서(공급 관계에서의 예 : 공급자 주문, 부품, 프로젝트, 수량)를 기억하는데 부담이 없어야 합니다. 따라서 우리는 도메인 순서가 있는 관계가 아닌 순서가 없는 동일한 도메인을 갖는 관계(relationships)를 제안합니다. 이를 위해 도메인은 적어도 주어진 관계 내에서 위치를 사용하지 않고 고유하게 식별 가능해야 한다. 따라서 두 개 이상의 동일한 도메인이 있는 경우, 우리는 각각의 경우에 도메인 이름이 주어진 관계에서 해당 도메인이 수행하는 역할을 식별하는 고유한 역할 이름으로 한정되어야 한다. 예를 들어, 그림 2의 관계 구성요소에서 첫 번째 도메인 부품은 서브, 두 번째 도메인 부품은 수퍼로 한정될 수 있으므로 사용자는 이러한 도메인 간의 순서와 관계없이 관계(relationship) 구성요소와 해당 도메인(sub.part super.part, 수량)을 처리할 수 있습니다.
  
 요약하면, 대부분의 사용자는 시간 변동 관계(= relationships)(relation 대신에) 모음으로 구성된 데이터의 관계형 모델과 상호 작용해야 한다고 제안합니다. 각 사용자는 도메인 이름(필요할 때마다 역할이 한정됨)과 함께 이름 이상의 관계(relationship)를 알 필요가 없습니다. 이 정보조차도 사용자의 요청에 따라 시스템에 의해 메뉴 스타일로 제공될 수 있습니다(보안 및 개인 정보 제한에 따라).
 
  The term relation is used here in its accepted mathematical sense. Given sets S1, S2, ···, Sn, (not necessarily distinct), R is a relation on these n sets if it is a set of n-tuples each of which has its first element from S1, its second element from S2, and so on. We shall refer to Sj as the jth domain of R. As defined above, R is said to have degree n. Relations of degree 1 are often called unary, degree 2 binary, degree 3 ternary, and degree n n-ary.
  
 For expository reasons, we shall frequently make use of an array representation of relations, but it must be remembered that this particular representation is not an essential part of the relational view being expounded. An array which represents an n-ary relation R has the following properties:
 
1. Each row represents an n-tuple of R.   
2. The ordering of rows is immaterial.   
3. All rows are distinct.   
4. The ordering of columns is significant - it corresponds to the ordering S1, S1, ···, Sn of the domains on which R is defined (see, however, remarks below on domain-ordered and domain-unordered relations ).   
5. The significance of each column is partially conveyed by labeling it with the name of the corresponding domain.   
 The example in Figure 1 illustrates a relation of degree called supply, which reflects the shipments-in-progress of parts from specified suppliers to specified projects in specified quantities.   

supply	supplier	part	project	quantity
	1	2	5	17
	1	3	5	23
	2	3	7	9
	2	7	5	4
	4	1	1	12
 - Figure 1. A relation of degree 4

 One might ask: If the columns are labeled by the name of corresponding domains, why should the ordering of columns matter? As the example in Figure 2 shows, two columns may have identical headings (indicating identical domains ) but possess distinct meanings with respect to the relation. The relation depicted is called component. It is a ternary relation, whose first two domains are called part and third domain is called quantity. The meaning of component (x, y, z) is that part x is an immediate component (or subassembly ) of part y, and z units of part x are needed to assemble one unit of part y. It is a relation which plays a critical role in the parts explosion problem.

component	(part	part	quantity
	1	5	9
	2	5	7
	3	5	2
	2	6	12
	3	6	3
	4	7	1
	6	7	1
			
- Figure 2. A relation with two identical domains
 
 It is a remarkable fact that several existing information systems (chiefly those based on tree-structured files) fail to provide data representations for relations which have two or more identical domains. The present version of IMS/360 [5] is an example of such a system.
 
The totality of data in a data bank may be viewed as a collection of time-varying relations. These relations are of assorted degrees. As time progresses, each n-ary relation may be subject to insertion of additional n-tuples, deletion of, existing ones, and alteration of components of any of its existing n-tuples.

In many commercial, governmental, and scientific data banks, however, some of the relations are of quite high degree (a degree of 30 is not at all uncommon). Users should not normally be burdened with remembering the domain ordering of any relation (for example, the ordering supplier, then part, then project, then quantity in the relation supply). Accordingly, we propose that users deal, not with relations which are domain-ordered, but with relationships which are their domain-unordered counterparts. To accomplish this, domains must be uniquely identifiable at least within any given relation, without using position. Thus, where there are two or more identical domains, we require in each case that the domain name be qualified by a distinctive role name, which serves to identify the role played by that domain in the given relation. For example, in the relation component of Figure 2, the first domain part might be qualified by the role name sub, and the second by super, so that users could deal with the relationship component and its domains - sub.part super.part, quantity - without regardto any ordering between these domains.

To sum up, it is proposed that most users should interact with a relational model of the data consisting of a collection of time-varying relationships (rather than relations). Each user need not know more about any relationship than its name together with the names of its domains (role qualified whenever necessary).[see note 3] Even this information might be offered in menu style by the system (subject to security and privacy constraints ) upon request by the user.

 There are usually many alternative ways in which a relational model may be established for a data bank. In order to discuss a preferred way (or normal form, we must first introduce a few additional concepts (active domain, primary key, foreign key, nonsimple domain) and establish some links with terminology currently in use in information systems programming. In the remainder of this paper, we shall not bother to distinguish between relations and relationships except where it appears advantageous to be explicit.
 
Consider an example of a data bank which includes relations concerning parts, projects, and suppliers. One relation called part is defined on the following domains:

1. part number   
2. path name   
3. part color   
4. part weight   
5. quantity on hand   
6. quantity on order   
and possibly other domains as well. Each of these domains is, in effect, a pool of values, some or all of which may be represented in the data bank at any instant. While it is conceivable that at some instant, all part colors are present, it is unlikely that all possible part weights, part names, and part numbers are. We shall call the set of values represented at some instant the active domain at that instant.

Normally, one domain (or combination of domains) of a given relation has values which uniquely identify each element (n-tuple) of that relation. Such a domain (or combination) is called a primary key. In the example above, part number would be a primary key, while part color would not be. A primary key is nonredundant if it is either a simple domain (not a combination) or a combination such that none of the participating simple domains is superfluous in uniquely identifying each element. A relation may possess more than one nonredundant primary key. This would be the case in the example if different parts were always given distinct names. Whenever a relation has two or more nonredundant primary keys, one of them is arbitrarily selected and called the primary key of that relation.

A common requirement is for elements of a relation to cross-reference other elements of the same relation or elements of a different relation. Keys provide a user-oriented means (but not the only means) of expressing such cross-references. We shall call a domain (or domain combination) of relation R a foreign key if it is not the primary key of R but its elements are values of the primary key of some relation S (the possibility that S and R are identical is not excluded). In the relation supply of Figure 1, the combination of supplier, part, project is the primary key, while each of these three domains taken separately is a foreign key.

In previous work there has been a strong tendency to treat the data in a data bank as consisting of two parts, one part consisting of entity descriptions (for example, descriptions of suppliers) and the other part consisting of relations between the various entities or types of entities (for example, the supply relation). This distinction is difficult to maintain when one may have foreign keys in any relation whatsoever. In the user's relational model there appears to be no advantage to making such a distinction (there may be some advantage, however, when one applies relational concepts to machine representations of the user's set of relationships).

So far, we have discussed examples of relations which are defined on simple domainsÑdomains whose elements are atomic (nondecomposable) values. Nonatomic values can be discussed within the relational framework. Thus, some domains may have relations as elements. These relations may, in turn, be defined on nonsimple domains, and so on. For example, one of the domains on which the relation employee is defined might be salary history. An element of the salary history domain is a binary relation defined on the domain date and the domain salary. The salary history domain is the set of all such binary relations. At any instant of time there are as many instances of the salary history relation in the data bank as there are employees. In contrast, there is only one instance of the employee relation.

The terms attribute and repeating group in present data base terminology are roughly analogous to simple domain and nonsimple domain, respectively. Much of the confusion in present terminology is due to failure to distinguish between type and instance (as in "record") and between components of a user model of the data on the one hand and their machine representation counterparts on the other hand (again, we cite "record" as an example).

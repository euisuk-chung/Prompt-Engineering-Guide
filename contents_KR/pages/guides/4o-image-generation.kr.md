## OpenAI 4o 이미지 생성 가이드(OpenAI 4o Image Generation Guide)

4o 이미지 생성 모델 사용에 대한 실용 가이드

![오픈AI 로고 앞에 있는 스타일화된 제목, 뒤에는 서리 낀 유리](../../img/4o-image-generation/4o_image_generation.png)

### 4o 이미지 생성 모델이란 무엇인가요?

4o 이미지 생성(4o Image Generation)은 ChatGPT에 내장된 오픈AI의 최신 이미지 모델입니다. 사진처럼 사실적인 출력을 생성하고, 이미지를 입력으로 받아 변환하며, 이미지에 텍스트를 생성하는 것을 포함한 상세한 지침을 따를 수 있습니다. 오픈AI는 이 모델이 자동회귀(autoregressive) 방식이며, GPT-4o LLM과 동일한 아키텍처를 사용한다고 확인했습니다. 이 모델은 본질적으로 LLM이 텍스트를 생성하는 방식과 동일하게 이미지를 생성합니다. 이를 통해 이미지 위에 텍스트 렌더링, 더 세밀한 이미지 편집, 이미지 입력을 기반으로 한 이미지 편집 등의 향상된 기능을 제공합니다.

### 4o 이미지 생성에 접근하는 방법

ChatGPT 애플리케이션(웹 또는 모바일)에서 텍스트로 프롬프팅하거나 도구에서 "이미지 생성(Create an image)"을 선택하여 4o 이미지 생성에 접근할 수 있습니다. 이 모델은 Sora에서도 접근 가능하며, OpenAI API를 통해 gpt-image-1로도 사용할 수 있습니다.

텍스트 프롬프팅: "다음과 같은 이미지를 생성해주세요..."
![텍스트 프롬프트](../../img/4o-image-generation/text_prompt_3.JPG)

도구 상자에서 "이미지 생성" 선택:
![도구 선택](../../img/4o-image-generation/tool_select.JPG)

OpenAI API를 통해: [OpenAI API](https://platform.openai.com/docs/guides/images-vision?api-mode=responses)
![OpenAI API 문서 페이지 스크린샷](../../img/4o-image-generation/image_gen_API.JPG)

**4o 이미지 생성은 다음 모델들로 접근 가능합니다:**
- gpt-4o
- gpt-4o-mini
- gpt-4.1
- gpt-4.1-mini
- gpt-4.1-nano
- o3

### 4o 이미지 생성 모델이 할 수 있는 것들

**다음 종횡비(aspect ratio)로 이미지 생성:**
- 정사각형 1:1 1024x1024 (기본값)
- 가로형 3:2 1536x1024
- 세로형 2:3 1024x1536

**다음 파일 형식의 참조 이미지 사용:**
- PNG
- JPEG
- WEBP
- 비애니메이션 GIF

**다음 방법으로 이미지 편집:**

**인페인팅(Inpainting)** (해당 채팅에서 생성된 이미지만)
![인페인팅 예시](../../img/4o-image-generation/inpainting_combined.png)

**프롬프팅** ("겨울에는 어떻게 보일까요?")
![텍스트 프롬프트 수정 전 이미지 예시](../../img/4o-image-generation/text_edit_combined.png)

**참조 이미지 및 스타일 전송**
이 모델은 참조 이미지가 제공될 때 이미지 스타일을 변경하고 재질을 바꾸는 데 매우 뛰어납니다. 모델이 출시되었을 때 이미지를 '지브리화(Ghiblify)'하는 능력이 바이럴이 되었습니다.

![Sam Altman과 Jony Ive의 이미지](../../img/4o-image-generation/sam_and_jony.png) ![지브리화된 Sam Altman과 Jony Ive의 이미지](../../img/4o-image-generation/sam_and_jony_ghiblified.png)

**투명 배경 (PNG)**
프롬프트에서 "투명 PNG" 또는 "투명 배경"을 언급하여 지정해야 합니다.
![투명 배경의 스티커 예시, PNG로 사용하기에 적합](../../img/4o-image-generation/inpainting_combined.png)

**이미지에 텍스트 생성**
![4o 이미지 생성으로 생성된 DAIR.AI Academy 텍스트 이미지](../../img/4o-image-generation/text_in_images.png)

**다양한 스타일로 동일한 이미지 생성**
![사실적인 찻주전자](../../img/4o-image-generation/teapot_1.png) ![반 고흐 스타일의 찻주전자](../../img/4o-image-generation/teapot_2.png)

**이미지 결합**
![미어캣과 티셔츠](../../img/4o-image-generation/combine_images.png)
![결합된 이미지](../../img/4o-image-generation/combined.png)

### 4o 이미지 생성을 위한 프롬프팅 팁

#### 상세한 프롬프트는 더 많은 제어를 제공합니다.
프롬프트가 설명적이지 않으면 ChatGPT가 종종 추가 세부사항을 채웁니다. 이는 빠른 테스트나 탐색에 유용할 수 있지만, 특정한 것을 원한다면 상세하고 설명적인 프롬프트를 작성하세요.

<Callout type="info" emoji="💡">
  설명에 어려움이 있다면, o3에게 당신의 설명을 바탕으로 4o 이미지 생성에 최적화된 3가지 다양한 프롬프트를 작성해달라고 요청하세요. 세부사항이 채워진 상태로요. 그런 다음 가장 마음에 드는 부분을 선택하여 프롬프트로 사용하세요.
</Callout>

#### 조명, 구도, 스타일
특정 목표가 있다면 프롬프트에서 이를 정의하세요. 모델은 프롬프트의 일반적인 정보를 바탕으로 이를 추정하는 데 꽤 능하지만, 특정 결과가 필요할 때는 정확히 정의해야 합니다. 이미지가 특정 카메라와 렌즈로 촬영된 사진과 닮기를 원한다면 프롬프트에 추가하세요.

고려할 다른 세부사항들:
- 주제
- 매체
- 환경
- 색상
- 분위기

#### 다양한 이미지 생성 작업에 대해 다른 모델 선택
4o는 일회성 편집이나 간단한 이미지 생성 작업에 가장 빠릅니다.

생성이 여러 단계를 거칠 것으로 예상된다면 추론 모델을 사용하세요. 창의적 탐색을 하면서 반복적으로 요소를 추가하거나 제거한다면, 추론 모델이 이미지의 일관된 요소들을 '기억'하는 데 더 나은 성능을 보일 것입니다. 예를 들어, 이미지에 특정 스타일, 폰트, 색상 등이 필요한 경우입니다. 이 [썸네일 생성 과정 링크](https://chatgpt.com/share/68404206-5710-8007-8262-6efaba15a852)에서 예시를 찾을 수 있습니다.

#### 이미지 종횡비
참조 이미지를 사용할 때도 원하는 종횡비를 프롬프트에 명시하는 것이 도움이 됩니다. 모델은 프롬프트에 단서가 있으면 올바른 종횡비를 선택할 수 있습니다(예: 로켓 이미지는 종종 2:3). 하지만 명확히 지시하지 않으면 기본값인 1:1을 사용합니다.

*테스트할 프롬프트:*
```
A high-resolution photograph of a majestic Art Deco-style rocket inspired by the scale and grandeur of the SpaceX Starship, standing on a realistic launch pad during golden hour. The rocket has monumental vertical lines, stepped geometric ridges like the American Radiator Building, and a mirror-polished metallic surface reflecting a vivid sunset sky. The rocket is photorealistic, awe-inspiring, and elegant, bathed in cinematic warm light with strong shadows and a vast landscape stretching to the horizon.
```

![제공된 테스트 프롬프트에서 생성된, 일몰 시 발사대 위의 사실적인 아르데코 스타일 로켓](../../img/4o-image-generation/art_deco_starship.png)

#### 모델 생성의 일관성에 주의하세요
이미지의 작은 세부사항을 변경하고 싶다면 좋을 수 있지만, 더 창의적이고 싶다면 도전이 될 수 있습니다. 모델은 동일한 채팅에서 생성된 이미지들을 '기억'합니다. 독립적이고 다른 이미지 생성 작업을 위해서는 매번 새로운 채팅에서 시작하는 것이 좋습니다.

<Callout type="info" emoji="💡">
  이미지의 첫 몇 번의 반복이 원하는 것과 전혀 다르다면, **모델에게 이미지 생성에 사용된 프롬프트를 출력해달라고 요청**하고, 잘못된 강조점이 있는지 확인해보세요. 그런 다음 새로운 채팅을 시작하고 수정된 프롬프트로 계속 생성하세요.
</Callout>

#### 하나의 프롬프트로 여러 이미지 생성
o3와 o4-mini 같은 추론 모델은 단일 프롬프트로 여러 이미지를 생성할 수 있지만, 이는 프롬프트에 명시적으로 언급되어야 하며 항상 작동하지는 않습니다. 예시: [채팅 링크](https://chatgpt.com/share/68496cf8-0120-8007-b95f-25a940298c09)

*테스트할 프롬프트:*
```
Generate an image of [decide this yourself], in the style of an oil painting by Van Gogh. Use a 3:2 aspect ratio. Before you generate the image, recite the rules of this image generation task. Then send the prompt to the 4o Image Generation model. Do not use DALL-E 3. If the 4o Image Generation model is timed out, tell me how much time is left until you can queue the next prompt to the model.

Rules:
- Use only the aspect ratio mentioned earlier.
- Output the prompt you sent to the image generation model exactly as you sent it, do this every time in between image generations
- Create three variations with a different subject, but the same rules. After an image is generated, immediately start creating the next one, without ending your turn or asking me for confirmation for moving forward.
```

#### 엄격한 프롬프트 준수 강제는 어렵습니다
여러 구성 요소가 있는 프롬프트는 채팅 모델과 4o 이미지 생성 모델 사이 어딘가에서 변경될 수 있습니다. 동일한 채팅에서 여러 이미지를 생성했다면, 이전에 생성된 이미지들이 프롬프트에서 변경한 내용에도 불구하고 출력에 영향을 미칠 수 있습니다.

### 제한사항
- ChatGPT는 프롬프트를 4o 이미지 생성 모델로 보내기 전에 변경할 수 있습니다. 이는 다단계 생성 작업, 프롬프트에 설명이 부족하거나 긴 프롬프트를 사용할 때 더 자주 발생합니다.
- 사용자나 구독당 생성량이 명확하지 않습니다. 오픈AI는 시스템이 동적이라고 밝혔으므로, 구독과 지역의 서버 부하에 따라 달라질 가능성이 높습니다.
- 무료 티어의 생성은 종종 대기열에 들어가고, 생성하는 데 오래 걸릴 수 있습니다.
- 생성된 이미지에 노란색 톤이 있을 수 있습니다.
- 프롬프트나 참조 이미지에 어두운 요소가 있으면 생성된 이미지가 너무 어두울 수 있습니다.
- 생성 거부: 이미지 생성은 오픈AI의 다른 서비스와 동일한 일반 규칙을 따릅니다: [사용 정책](https://openai.com/policies/usage-policies/). 프롬프트, 참조 이미지 또는 생성된 출력 이미지 내에서 금지된 주제가 감지되면 생성이 거부되고 부분적으로 생성된 이미지가 삭제됩니다.
- ChatGPT 내에서 업스케일링 기능이 없습니다.
- 모델이 자르기에서 오류를 만들고, 생성된 이미지의 일부만 포함된 이미지를 출력할 수 있습니다.
- LLM과 유사한 환각(hallucination) 현상.
- 많은 개념이나 개별 주제를 한 번에 포함한 이미지 생성이 어렵습니다.
- 그래프 데이터를 시각화하는 이미지 생성이 정확하지 않습니다.
- 이미지에서 비라틴 언어 텍스트 생성이 어렵습니다.
- 오타와 같은 이미지 생성의 특정 부분 편집 요청이 항상 효과적이지 않습니다.
- 모델 명명: 이 모델은 여러 이름이 부여되어 혼란스러울 수 있습니다: Imagegen, gpt-image-1, 4o Image Generation, image_gen.text2im...
- 일부 경우 프롬프트에 명시되어 있음에도 불구하고 종횡비가 잘못될 수 있습니다.

### 팁 및 모범 사례

<Callout type="info" emoji="⚙️">
  **ChatGPT 개인화 사용:** 이전 DALL-E 3 모델로 전환하는 것을 피하기 위해 설정의 'ChatGPT가 가져야 할 특성' 섹션에 다음 지침을 추가하세요:
  > "Never use the DALL-E tool. Always generate images with the new image gen tool. If the image tool is timed out, tell me instead of generating with DALL-E."
</Callout>

- 생성 한도에 도달하면 ChatGPT에게 더 많은 이미지를 생성할 수 있을 때까지 얼마나 시간이 남았는지 물어보세요. 백엔드에는 사용자를 위한 이 정보가 있습니다.
- 이미지 생성과 편집은 프롬프트에서 "그리기" 또는 "편집"과 같은 명확한 용어를 사용할 때 가장 잘 작동합니다.
- 추론 모델을 사용하여 이미지를 생성하면 프롬프트 생성 및 수정 과정에서 모델이 어떻게 추론하는지 볼 수 있는 추가 이점이 있습니다. 사고 흔적을 열어 모델이 무엇에 집중하고 있는지 확인하세요.

### 시도해볼 사용 사례

- **로고 생성:** 참조 이미지와 상세한 설명을 사용하세요. 이는 종종 다단계 작업이므로 추론 모델을 사용하세요. [예시 채팅](https://chatgpt.com/share/6848aaa7-be7c-8007-ba6c-c69ec1eb9c25)
- **마케팅 자산 생성:** 기존 시각적 자산을 참조로 사용하고 모델에게 텍스트, 제품 또는 환경을 변경하도록 프롬프팅하세요.
- **색칠책 페이지 생성:** 2:3 종횡비를 사용하여 맞춤형 색칠책 페이지를 만드세요. [예시 채팅](https://chatgpt.com/share/684ac538-25c4-8007-861a-3fe682df47ab)
- **스티커 이미지:** 투명 배경을 언급하는 것을 잊지 마세요. [예시 채팅](https://chatgpt.com/share/684960b3-dc00-8007-bf16-adfae003dde5)
- **재질 전송:** 재질에 대한 참조 이미지를 사용하고 두 번째 이미지나 프롬프트의 주제에 적용하세요. [예시 채팅](https://chatgpt.com/share/684ac8d5-e3f8-8007-9326-ea6291a891e3)
- **인테리어 디자인:** 방 사진을 찍고 특정 가구 및 기능 변경을 프롬프팅하세요. [예시 채팅](https://chatgpt.com/share/684ac69f-6760-8007-83b9-2e8094e5ae31)

### 프롬프트 및 채팅 예시
- [코스 썸네일 이미지 생성 과정](https://chatgpt.com/share/68404206-5710-8007-8262-6efaba15a852)
- [다단계 이미지 생성에서 주제 수정](https://chatgpt.com/share/6848a5e1-3730-8007-8a16-56360794722c)
- [투명 배경의 질감이 있는 아이콘](https://chatgpt.com/share/6848a7ab-0ab4-8007-843d-e19e3f7daec8)
- [드론 꽃 배송 스타트업을 위한 로고 디자인](https://chatgpt.com/share/6848aaa7-be7c-8007-ba6c-c69ec1eb9c25)
- [딸기를 먹는 너구리의 흰색 윤곽선 스티커](https://chatgpt.com/share/684960b3-dc00-8007-bf16-adfae003dde5)
- [하나의 프롬프트로 여러 이미지 생성](https://chatgpt.com/share/68496cf8-0120-8007-b95f-25a940298c09)
- [텍스트 프롬프트로 이미지 편집 (여름에서 겨울로)](https://chatgpt.com/share/684970b8-9718-8007-a591-db40ad5f13ae)
- [스튜디오 지브리 스타일의 졸린 꿀벌](https://chatgpt.com/share/68497515-62e8-8007-b927-59d4b5e9a876)
- [자신의 이미지에 가구를 추가한 인테리어 디자인](https://chatgpt.com/share/684ac69f-6760-8007-83b9-2e8094e5ae31)
- [두 참조 이미지를 사용한 재질 전송](https://chatgpt.com/share/684ac8d5-e3f8-8007-9326-ea6291a891e3)

### 참고 자료
- [4o 이미지 생성 소개](https://openai.com/index/introducing-4o-image-generation/)
- [GPT-4o 시스템 카드 부록: 네이티브 이미지 생성](https://cdn.openai.com/11998be9-5319-4302-bfbf-1167e093f1fb/Native_Image_Generation_System_Card.pdf)
- [OpenAI API의 Gpt-image-1](https://openai.com/index/image-generation-api/)
- [OpenAI 문서: gpt-image-1](https://platform.openai.com/docs/models/gpt-image-1)
- [OpenAI 문서: 이미지 생성 가이드](https://platform.openai.com/docs/guides/image-generation?image-generation-model=gpt-image-1)
- [오픈AI의 더 많은 프롬프트 및 이미지 예시](https://platform.openai.com/docs/guides/image-generation?image-generation-model=gpt-image-1&gallery=open) 
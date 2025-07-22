做一个label太长了，可以放的地方放不下，而自动滚动的feature。
粥里面就有，技能太长了会scroll一下。

原来是手搓。
有两个text component。
一个普通的label，用于短的时候居中。
一个长一点的label，一个container加上mask，遮罩text component，自带滚动代码。
然后用content size fitter来看让哪个setActive哪个不用。

然后写滚动代码的时候算不好什么时候停，什么时候重新切回头再滚动。

deepseek说为什么不用scrollView结构直接搞定scroll这一部分呢。

'''
using UnityEngine;
using UnityEngine.UI;
using System.Collections;

public class ConstantSpeedScroll : MonoBehaviour
{
    public ScrollRect scrollRect;
    public float scrollSpeed = 50f; // 像素/秒
    public float startDelay = 1f;
    public float endDelay = 1f;

    private RectTransform content;
    private float contentHeight;
    private float viewportHeight;
    private bool isScrolling = false;

    void Start()
    {
        content = scrollRect.content;
        StartCoroutine(StartScrolling());
    }

    IEnumerator StartScrolling()
    {
        yield return new WaitForEndOfFrame();
        
        // 获取内容高度和视口高度
        contentHeight = content.rect.height;
        viewportHeight = scrollRect.viewport.rect.height;
        
        // 只有当内容超过视口时才滚动
        if (contentHeight > viewportHeight)
        {
            StartCoroutine(ScrollContent());
        }
    }

    IEnumerator ScrollContent()
    {
        yield return new WaitForSeconds(startDelay);
        
        float scrollPosition = 0;
        float maxScrollDistance = contentHeight - viewportHeight;
        isScrolling = true;
        
        while (isScrolling)
        {
            // 计算新的滚动位置（基于像素）
            scrollPosition += scrollSpeed * Time.deltaTime;
            
            // 转换为normalizedPosition
            float normalizedPos = 1 - (scrollPosition / maxScrollDistance);
            scrollRect.verticalNormalizedPosition = Mathf.Clamp01(normalizedPos);
            
            // 到达底部时暂停并重置
            if (scrollPosition >= maxScrollDistance)
            {
                isScrolling = false;
                yield return new WaitForSeconds(endDelay);
                scrollRect.verticalNormalizedPosition = 1;
                yield return new WaitForSeconds(1f);
                scrollPosition = 0;
                isScrolling = true;
            }
            
            yield return null;
        }
    }
}
'''

这个版本跟自己改的有点不一样啦。用deepseek跑了两版融了一下…
没写两个StartCoroutine，写了一个:
while true -> if (!isPaused) -> scroll -> check verticalNormalizedPosition
这样的层。
scrollRect.vecticalNormalizedPosition是用比例计算的，方便也不方便!
deepseek说如果要固定scroll的速度(像素)那手动算一下比例吧。
我觉得可以。 

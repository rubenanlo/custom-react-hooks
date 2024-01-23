# My react custom hooks

This is a library of custom hooks that I have created to make them more accessible.

## usePagination:

Custom hook to add all the logic behind pagination.

Usage: In your component, add this:

```javascript
const Example = () => {
  const { currentPosts, ...pagination } = usePagination({
    initialNumberOfPages: 1, // this determines how many pages to collapse under the three dot symbol;
    itemsPerPage: 3, // How many items you want to show per single page
    items: articles,
  });

  return (
    <>
      {currentPosts.map((article) => (
        <Article key={article.slug} article={article} />
      ))}
      <Pagination pagination={pagination} />
    </>
  );
};
```

## useResponsive:

It returns a boolean stating whether the screen size threshold is met or not.

Usage:

```javascript
const Component = ({ articles }) => {
  const isSmallScreen = useResponsive(1024);

  return isSmallScreen ? (
    <Carousel articles={articles} />
  ) : (
    <ArticleList articles={articles} />
  );
};
```

## useAnimatedValue:

It adds a framer motion animation than makes a number count up. This hook goes with the helper function formatData.

```javascript
const blurAnimation = {
  initial: { filter: "blur(0.2rem)" },
  animate: { filter: "blur(0)" },
  transition: { duration: 0.5 },
};

const Component = ({ articles }) => {
  const animatedTotalPosts = useAnimatedValue({
    from: 0,
    to: totalPosts,
    formatThousands: true,
    animations: { duration: 0.5 },
  });

  return (
    <Container.Columns>
      <TextLayout.Number {...blurAnimation} className="text-7xl">
        {animatedTotalPosts}
      </TextLayout.Number>
    </Container.Columns>
  );
};
```

## useModalTooltip:

It renders a modal ShowTruncated showing full text of any truncated text in the site. The modal moves with the mouse position;

Usage:

```javascript
const Component = () => {
  const {
    fullText,
    modalVisibility,
    mousePosition,
    handleMouseEnter,
    handleMouseMove,
    handleMouseLeave,
  } = useModalTooltip();

  const maxCharacters = isSmallScreen ? 50 : 30;
  const setTruncatedText = (text) =>
    truncate(text, {
      length: maxCharacters,
      separator: " ",
    });

  return (
    <Container.Section className="w-auto">
      {cards.map(({ description }) => (
        <>
          <TextLayout.Paragraph
            paragraph={index % 2 ? description : setTruncatedText(description)}
            className=" text-sm mt-3"
            onMouseEnter={(e) => {
              handleMouseEnter(description, setTruncatedText(description), e);
            }}
            onMouseMove={handleMouseMove}
            onMouseLeave={handleMouseLeave}
          />
          <ShowTruncated
            value={fullText}
            isVisible={modalVisibility}
            mousePosition={mousePosition}
          />
        </>
      ))}
    </Container.Section>
  );
};
```

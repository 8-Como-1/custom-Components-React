import React, { useState, useRef, useEffect } from 'react';
import { Tooltip, Typography } from 'antd';
import './style.less';

const { Text, Paragraph } = Typography;

interface TextComponentProps {
  children: React.ReactNode;
  lineHeight?: number;
  defaultText?: string;
  type?: 'custom' | 'ant'; // 自定义方式实现，还是ant方式
  width?: number;
  rows?: number;
}

// 自定义的能解决百分之99的问题;
export const EllipsisText: React.FC<TextComponentProps> = ({
  children,
  lineHeight = 20,
  defaultText = '-',
  type = 'custom',
  width,
  rows,
}) => {
  const textNode =
    (typeof children === 'string' ? children.trim() : children) ||
    (children === 0 ? 0 : <span className="empty-text">{defaultText}</span>);
  const textRef = useRef<HTMLDivElement>(null);
  const [isOverflow, setIsOverflow] = useState(false);

  const handleIsOverflow = () => {
    if (textRef.current) {
      const { clientHeight, scrollHeight } = textRef.current;
      setIsOverflow(clientHeight < scrollHeight);
    }
  };

  useEffect(() => {
    const observer = new ResizeObserver(() => {
      handleIsOverflow();
    });
    if (textRef.current) {
      observer.observe(textRef.current);
    }
    return () => {
      if (textRef.current) {
        observer.unobserve(textRef.current);
      }
    };
  }, []);

  if (type === 'ant' || Number(rows) > 1) {
    return Number(rows) > 1 ? (
      <Paragraph
        ellipsis={{
          rows,
          expandable: 'collapsible',
        }}
      >
        {textNode}
      </Paragraph>
    ) : (
      <Text
        ellipsis={{
          tooltip: children,
        }}
      >
        {textNode}
      </Text>
    );
  }

  return (
    <div
      className="ellipsis-wrapper"
      style={
        width
          ? {
              width,
              maxWidth: width,
              minWidth: width,
            }
          : undefined
      }
    >
      <Tooltip title={isOverflow ? children : undefined}>
        <div className={'ellipsis-text'}>{textNode}</div>
      </Tooltip>
      <div
        ref={textRef}
        className={'ellipsis-none'}
        style={{ lineHeight: `${lineHeight}px`, maxHeight: lineHeight }}
      >
        {children}
      </div>
    </div>
  );
};

// export default Text;

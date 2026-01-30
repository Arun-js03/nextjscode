"use client";

import { useGSAP } from "@gsap/react";
import gsap from "gsap";
import Link from "next/link";
import type React from "react";
import { useRef } from "react";

interface AnimatedSocialIconProps {
  children: React.ReactNode;
  href: React.ComponentProps<typeof Link>["href"];
  className?: string;
}

export default function AnimatedSocialIcon({
  children,
  href,
  className,
}: AnimatedSocialIconProps) {
  const containerRef = useRef<HTMLAnchorElement>(null);

  useGSAP(
    () => {
      const paths = containerRef.current?.querySelectorAll(
        "path, circle, rect, line, polyline",
      );

      if (!paths) return;

      paths.forEach((path) => {
        const svgPath = path as SVGGeometryElement;
        if (svgPath.getTotalLength) {
          const length = svgPath.getTotalLength();
          // Store the length on the element for easy access
          svgPath.style.setProperty("--length", length.toString());
        }
      });
    },
    { scope: containerRef },
  );

  const handleMouseEnter = () => {
    const paths = containerRef.current?.querySelectorAll(
      "path, circle, rect, line, polyline",
    );
    if (!paths) return;

    paths.forEach((path) => {
      const svgPath = path as SVGGeometryElement;
      // Use property value we stored, or re-calculate if needed
      let length = parseFloat(svgPath.style.getPropertyValue("--length"));
      if (!length && svgPath.getTotalLength) {
        length = svgPath.getTotalLength();
      }

      if (length > 0) {
        gsap.fromTo(
          svgPath,
          { strokeDasharray: length, strokeDashoffset: length },
          {
            strokeDashoffset: 0,
            duration: 0.8,
            ease: "power2.out",
            overwrite: true,
          },
        );
      }
    });
  };

  const handleMouseLeave = () => {
    const paths = containerRef.current?.querySelectorAll(
      "path, circle, rect, line, polyline",
    );
    if (!paths) return;

    paths.forEach((path) => {
      const svgPath = path as SVGGeometryElement;
      gsap.to(svgPath, { strokeDashoffset: 0, duration: 0.3, overwrite: true });
    });
  };

  return (
    <Link
      ref={containerRef}
      href={href}
      target="_blank"
      rel="noopener noreferrer"
      className={className}
      onMouseEnter={handleMouseEnter}
      onMouseLeave={handleMouseLeave}
    >
      {children}
    </Link>
  );
}
